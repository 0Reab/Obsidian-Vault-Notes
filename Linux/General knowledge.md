### The difference between Linux and Unix.

Linux - is an open source OS with more  often updates.
Unix - Proprietary OS with slower updates and it's not free.

For example Linux is your ubuntu, kali or arch distros.
And Unix is being ran in data centers and big servers in a company.

### Init and systemd

Every time you boot your Linux machine here is what happens on a high level.
- BIOS loads the bootloader.
- Bootloader loads the kernel.
- Kernel loads the init or systemd.
- init or systemd load your services like networking and filesystem.

The difference between init and systemd is that init is a sequential service loader and systemd is parallel.
Init is also older than systemd.
You can interact with systemd services via systemctl to start/stop or see status of service.

### Inodes

What are inodes in linux you might ask?
Inodes are a data structure for linux filesystem.
It is possible to create a bunch of small files and have free disk space but at the same time be unable  to create a file.
What happened in that scenario is you run out of inodes.

Inodes contain the metadata of the file (permissions, ownership, timestamps, etc.) but not the filename itself.
Now this can happen if you made millions of 1 KB log files for example.

```shell
df -i # check inode usage
```

### Soft link and hard link

Soft and hard linking is done when you want to have a link to a file from some other place.
The difference between the soft and hard links:
- Hard link points directly to the original file on the disk.
- Soft link points to the file that points to the disk.

Meaning in the case of the soft link if the middle point between link and actual data on the disk is somehow broken, the link brakes as well.
On the other hand hard link will still point to the actual data regardless what happens to the file.
You can think of it as pointers if you are familiar with programming concepts.
One is a chain the other is not.