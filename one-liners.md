# linux-guides: one liners

<img align="right" alt="Console Icon" src="https://raw.githubusercontent.com/keithieopia/linux-guides/master/assets/console.png">

> one-liners collected through the times

These are short little chained commands that I find myself using time and time
again. Some use to be Bash aliases, but to be honest I always forgot what I
named them; thus this README was born instead.


## Files & Directories

### Find biggest files in a dir
```console
$ find . -type f -exec ls -s {} \; | sort -n -r | head -5
```

### Count the number of files in a dir
```console
$ ls -1 | wc -l
```

### Get a text file's character count
```console
$ wc -c "filename.txt"
```

### Get a text file's word count
```console
$ wc -w "filename.txt"
```

### Frequently used words in a text file

```console
$ <filename.txt tr -cs "[:alnum:]" "\n" | tr "[:lower:]" "[:upper:]" | awk '{h[$1]++}END{for (i in h){print h[i]" "i}}'|sort -nr | cat -n | head -n 10
```

*Replace:*

- `head -n 10` specifies the number of top words to show

### Get a random line from a file
```console
$ shuf -n 1 "filename.txt"
```

## Fix console gibberish
There are two people in this world:

1. Those that have accidentally `cat`'ed a binary and screwed up their $PS1
2. Those that have lied about doing the above out of shame

**Try any of the three below commands:**  
```console
$ reset
$ stty sane
$ echo -e "\033c"
```

*Tip:*

- The above are not needed if you simply need to `clear` the screen
- CTRL + L also clears the screen

## Fun

### Decode ROT13
```console
$ echo "Or Fher Gb Qevax Lbhe Binygvar" | tr A-Za-z N-ZA-Mn-za-m
```

*Replace:*  

- `Or Fher Gb Qevax Lbhe Binygvar` - the ciphertext to decode

### Act like you're very busy
```console
$ </dev/urandom hexdump -C | grep --color=auto "ca fe"
```

## Media conversion

### Repair corrupt videos / Fix "Build index then play" in VLC
This is often much faster and more accurate than VLC's built-in repair tool

```console
$ ffmpeg -i "badvideo.avi" -c:v copy -c:a copy "fixedvideo.avi"
```

### MP3 to Ogg
```console
$ ffmpeg -i "song.mp3" -c:a libvorbis -q:a 4 "${1%\.*}.ogg";
```

*Replace:*
- `song.mp3` - the MP3 to convert

### MIDI to Ogg

```console
$ fluidsynth -nli -r 48000 -o synth.cpu-cores=$(grep processor /proc/cpuinfo | wc -l) -F "/dev/shm/tmp-midi.raw" /usr/share/soundfonts/FluidR3_GM2-2.sf2 "filename.midi"
$ oggenc -r -B 16 -C 2 -R 48000 "/dev/shm/tmp-midi.raw" -o "filename.ogg"
$ rm "/dev/shm/tmp-midi.raw"
```

*Replace:*

- `/usr/share/soundfonts/FluidR3_GM2-2.sf2` - path to the SoundFont to use.
- `filename.midi` - MIDI file to convert
- `filename.ogg` - the desired converted file name


## Spring Cleaning

### clean journalctl, keeping only 1 week of logs
```console
$ journalctl --vacuum-time=1week
```


## Network

### Get machine's local IP
```console
$ ip -o addr show wlp1s0 | head -n 1 | sed 's/.*inet \(\S*\)\/.*/\1/g'
```

*Replace:*

1. `wlp1s0` - your network interface

### Get machine's public IP
```console
curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'
```

### Fix network down after suspend
For some reason, my network goes down on suspend. Most of the time the IP
address reservation has been lost (fix: `dhcpcd -d`), but sometimes the entire
networking stack gets borked. Restarted the systemD service usually fixes both
issues:

```console
$ systemctl restart netctl-auto@wlp1s0.service
```

*Replace:*

1. `wlp1s0` - your network interface


### Fast network partition cloning
By zeroing the slack space of the partition beforehand, we can compress the
unused space before transfer, reducing bandwidth and the time needed to transfer.

**1. Zero the slack space**  
On the computer that has the partition to copy:

```console
$ dd if=/dev/zero of=/mnt/copyme/bigfile.bin bs=1M
```

*Replace:*

-  `/mnt/copyme` - path of the mounted partition being copying

*Tip:*

- install and use `dcfldd` instead of `dd` for the current progress and a slight
speed improvement


**2. Unmount the partition**  
You'll get unexpected results if data is written to the partition during the
clone; usually mismatching MFTs. It's easier and safer just to unmount the
partition beforehand.

```
$ umount /mnt/copyme
```

**3. Clone the partition**  
On the computer receiving the cloned partition:

```console
dd if=~/cloned-partition.bin bs=1M | gzip -1 - | pv | ssh root@10.0.0.1 "gunzip -1 - | dd of=/dev/sdXn" bs=1M
```

*Replace:*

- `~/cloned-partition.bin` - where to save the cloned data, file or local partition
-  `10.0.0.1` - the remote IP of the computer with the partition to copy
- `/dev/sdXn` - the remote partition to copy, which was mounted to `/mnt/copyme`
in the previous examples

*Tip:*  
- `pv` will show the current progress, which is useful given partitions are
usually large. Ultimately can be excluded from the above command if not wanted


## Credits
Most of these commands are so well known, their original authors are unknown. If
you are the author or happen to know them please contact me or submit a pull
request so I can give credit.

* `midi2ogg` - Found on the [ArchWiki](https://wiki.archlinux.org/index.php/FluidSynth#How_to_convert_MIDI_to_OGG)  
  License: [GNU FDL](https://www.gnu.org/copyleft/fdl.html)

  *This is part of the [linux-guides](https://github.com/keithieopia/linux-guides) series. For more information, such see the [README](https://github.com/keithieopia/linux-guides/blob/master/README.md).*
