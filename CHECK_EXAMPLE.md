## BTRFS Scrubbing
btrfs will by default checksum your data blocks. \
Using the scrub feature, we can validate that there are no problems with any data blocks currently storing data. \
If any bad blocks are found under a disk image, you can just recover that image on other good sectors. \
To validate a single filesystem, do the following (replacing `/mnt/VAHAW81L` with your mounted btrfs)
```bash
btrfs scrub start /mnt/VAHAW81L
btrfs scrub status /mnt/VAHAW81L
```

If you have a microraids map file, you can use the provided scrub script to check all your filesystems.
```bash
./mr_scrub.sh <MAP> <start|status>

./mr_scrub.sh mnt_locations.map start
./mr_scrub.sh mnt_locations.map status
```

For each filesystem, it will give you relevant status.
```bash
Scrub started:    Tue Apr 14 12:53:58 2020
Status:           finished
Duration:         0:00:00
Total to scrub:   512.00KiB
Rate:             170.67KiB/s
Error summary:    no errors found
```

## RAID Checking
Another way to check your microraid is to ask the kernel to run an integrity check for you. \
This command can be given to each raid individually.
```bash
./mr_check.sh /dev/md/mynewraid
```

/proc/mdstat will show you the check status.
```bash
cat /proc/mdstat 
Personalities : [raid0] [raid1] [raid10] [raid6] [raid5] [raid4] 
md127 : active raid6 loop19[1] loop18[6] loop17[5] loop16[4] loop15[7] loop14[3] loop13[2] loop12[0]
      239797248 blocks super 1.2 level 6, 256k chunk, algorithm 2 [8/8] [UUUUUUUU]
      [====>................]  check = 23.3% (9324500/39966208) finish=8.8min speed=57308K/sec
```
