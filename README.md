# zfs-auto-snapshot

What has changed in this fork?
- By default, recursive snapshotting is not enabled. (Use TARGETS_REGULAR if the recursive flag is not set).
- Instead of UTC, use localtime.
- Subsequent snapshots will not be aborted even if the pre-snapshot command fails once.
- ~~By default, use `zfs_auto_snapshot_target` located in `/etc/profile` as the target. If this is not specified, then take snapshots of all datasets. with `//` (*e.g. `export zfs_auto_snapshot_target='tank1'`*).~~
  - It turns out setting `com.sun:auto-snapshot=true/false` with `zfs-auto-snapshot --default-exclude` is enough.
- Detect bytes_written firstly to prevent duplicate snapshot (and potential disk spin-ups)
- Add `-c` and `--default-exclude` options to default cron scripts
---

An alternative implementation of the zfs-auto-snapshot service for Linux
that is compatible with zfs-linux and zfs-fuse.

Automatically create, rotate, and destroy periodic ZFS snapshots. This is
the utility that creates the @zfs-auto-snap_frequent, @zfs-auto-snap_hourly,
@zfs-auto-snap_daily, @zfs-auto-snap_weekly, and @zfs-auto-snap_monthly
snapshots if it is installed.

This program is a posixly correct bourne shell script.  It depends only on
the zfs utilities and cron, and can run in the dash shell.


Installation:
-------------

~~wget https://github.com/zfsonlinux/zfs-auto-snapshot/archive/upstream/1.2.4.tar.gz  
tar -xzf 1.2.4.tar.gz  
cd zfs-auto-snapshot-upstream-1.2.4  
make install~~

```
git clone git@github.com:GrayXu/zfs-auto-snapshot.git
cd zfs-auto-snapshot
make install
```