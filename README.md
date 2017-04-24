# mount-data-disks

Automatically format and mount data disks on virtual machine instances.

This role is made of a script `mount-data-disks` that:
- Looks for data disks (disks larger than a given size
that are not already mounted and that do not exist in /etc/fstab)
- Format the eligible disks with a single ext4 partition if the disk is unformatted
- Mount the disks on /data-0, data-1, ... mountpoints
- Create subdirectories and set permissions on the mountpoints

## Usage

The script `mount-data-disks` is automatically launched at startup (via systemd).
Indeed, the mount is temporary: nothing is ever written in /etc/fstab.

You may run the commands `mount-data-disks` and `umount-data-disks` to
respectively mount and un-mount the data disks manually.


## Requirements

The scripts run with bash. They have been tested on Ubuntu 16.04+ only so far.
The only true requirement is a systemd compatible system to call the
command `mount-data-disks` at boot time.


## Naming

The mountpoints (/data-X) begin at 0 and rise incrementally,
in the order of the eligible device blocks.
Here is an example output of `df -h` on a machine configured with this role:
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        29G   19G  9.9G  66% /
/dev/sdb1        55G  577M   52G   2% /mnt
/dev/sdc1      1007G  351G  606G  37% /data-0
/dev/sde1        10G    1G    9G  10% /boot
/dev/sdf1       504G  226G  253G  48% /data-1
/dev/sdg1       504G  187G  292G  39% /data-2
```


## Variables

```yamlex
# File keeping track of mounted disks
mdd_file: /var/run/data-mountpoints

# Subdirectories to create on every mounted disk
mdd_subdirectories: []

# Owner and group of the mounted disks
mdd_final_user: root
mdd_final_group: root

# Minimum disk size in bytes (default: 100GB)
mdd_min_disk_size: 100122547200

```

## License

MIT

