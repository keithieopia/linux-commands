# Fast network partition cloning
By zeroing the slack space of the partition beforehand, we can compress the unused space before transfer, reducing bandwidth and the time needed to transfer.

## On the Computer that has the partition to copy:

### 1. Zero the slack space
Make sure the partition to clone **is** mounted, then use `dd` to completely fill the partition:

```console
mount /dev/your-partition /mnt/your-mounted-partition  
dd if=/dev/zero of=/mnt/your-mounted-partition/bigfile.bin bs=1M  
sync  
rm /mnt/your-mounted-partition/bigfile.bin
```

*Tip: install and use `dcfldd` instead of `dd` for the current progress and a slight speed improvement*


### 2. Unmount the partition
You'll get unexpected results if data is written to the partition during the clone; usually mismatching MFTs. It's easier and safer just to unmount the partition beforehand

```console
umount /mnt/your-mounted-partition
```

## On the Computer Receiving the Copy of the Partition

### 3. Clone the partition

```console
dd if=~/cloned-partition.bin bs=1M | gzip -1 - | pv | ssh user@copy-me-machine "gunzip -1 - | dd of=/dev/your-partition" bs=1M
```

*Tip: `pv` will show the current progress, which is useful given partitions are usually large. Ultimately can be excluded from the above command if not wanted*
