# linux-commands


## Files & directories 

### Find biggest files in a dir
```console
$ find . -type f -exec ls -s {} \; | sort -n -r | head -5
```

### Count the number of files in a dir
```console
$ ls -1 | wc -l
```

## Text files

### Character count
```console
$ wc -c "filename.txt"
```

### Word Count
```console
$ wc -w "filename.txt"
```

### Frequently used words
> Replace the "10" in `head -n 10` to specify the *number* of top words to show
```console
$ <filename.txt tr -cs "[:alnum:]" "\n" | tr "[:lower:]" "[:upper:]" | awk '{h[$1]++}END{for (i in h){print h[i]" "i}}'|sort -nr | cat -n | head -n 10
```

### Get a random line from a file
```console
$ shuf -n 1 "filename.txt"
```

## Security

### Generate a random password

```console
$ echo $(</dev/urandom tr -cd '[:alnum:]' | head -c ${1:-8})
```

**with special characters:**
```console
$ echo $(</dev/urandom tr -cd '[:graph:]' | head -c ${1:-8})
```

> Default length is 8 characters, which can be changed  in `head -c ${1:-8}`
> e.g.: `head -c ${1:-12}` for 12 characters

> `echo $()` is simply a wrapper for the real command, so that it gets outputted
> on a new line and doesn't mess up the prompt

## Fun

### Decode ROT13
> Replace "Or Fher Gb Qevax Lbhe Binygvar" with the ciphertext to decode 
```console
$ echo "Or Fher Gb Qevax Lbhe Binygvar" | tr A-Za-z N-ZA-Mn-za-m
```

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
> replace "song.mp3" with the file to convert
```console
$ ffmpeg -i "song.mp3" -c:a libvorbis -q:a 4 "${1%\.*}.ogg";
```

### MIDI to Ogg
> The `/usr/share/soundfonts/FluidR3_GM2-2.sf2` path may be different on your
> system, or you want to use a different soundfont. 
> the location of the SoundFont to use on your system.

> Replace "filename.midi" with the MIDI file to convert and "filename.ogg" with
> the desired converted file name
```console
$ fluidsynth -nli -r 48000 -o synth.cpu-cores=$(grep processor /proc/cpuinfo | wc -l) -F "/dev/shm/tmp-midi.raw" /usr/share/soundfonts/FluidR3_GM2-2.sf2 "filename.midi"
$ oggenc -r -B 16 -C 2 -R 48000 "/dev/shm/tmp-midi.raw" -o "filename.ogg"
$ rm "/dev/shm/tmp-midi.raw"
```

## Spring Cleaning

### clean journalctl, keeping only 1 week of logs
```console
$ journalctl --vacuum-time=1week
```


## Network

### Get machine's local IP 
> replace "wlp1s0" with your network interface

```console
$ ip -o addr show wlp1s0 | head -n 1 | sed 's/.*inet \(\S*\)\/.*/\1/g'
```

### Get machine's public IP
```console
curl -s checkip.dyndns.org | sed -e 's/.*Current IP Address: //' -e 's/<.*$//'
```

### Fix network down after suspend
> replace `wlp1s0` with your network interface's name
```console
$ systemctl restart netctl-auto@wlp1s0.service
```


### Fast network partition cloning
By zeroing the slack space of the partition beforehand, we can compress the 
unused space before transfer, reducing bandwidth and the time needed to transfer.

#### 1. Zero the slack space
On the computer that has the partition to copy:

> change `/mnt/copyme` to the location of your mounted partition you're copying

```console
$ dd if=/dev/zero of=/mnt/copyme/bigfile.bin bs=1M
```

> **tip:** install and use `dcfldd` instead of `dd` for the current progress and 
> a slight speed improvement


#### 2. Unmount the partition
You'll get unexpected results if data is written to the partition during the 
clone; usually mismatching MFTs. It's easier and safer just to unmount the 
partition beforehand.

```
$ umount /mnt/copyme
```

#### 3. Clone the partition
On the computer receiving the cloned partition:

> `~/cloned-partition.bin` - where to save the cloned data, may be a file or a 
> local partition

> `10.0.0.1` - the remote IP of the computer with the partition to copy

> `/dev/sda1` - the remote partition to copy, which was mounted to `/mnt/copyme` 
> in the above example

```console
dd if=/local/hdd/partition bs=1M | gzip -1 - | pv | ssh root@10.0.0.1 "gunzip -1 - | dd of=/remote/hdd/partition" bs=1M
```

> **tip:** `pv` will show the current progress, but ultimately can be excluded 
> from the above command if not wanted



## License
Copyright &copy; 2016 Timothy Keith

Licensed under the [GNU Free Documentation License 1.3 or later](https://github.com/keithieopia/linux-commands/blob/master/LICENSE).

The content is provided "as is" without warranty of any kind, either expressed 
or implied, including, but not limited to, the implied warranties of correctness 
and relevance to a particular subject. The entire risk as to the quality and 
accuracy of the content is with you. Should the content prove substandard, you 
assume the cost of all necessary servicing, repair, or correction. 
