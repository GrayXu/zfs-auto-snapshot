# zfs-auto-snapshot

this branch is for my usage. I use `--changed` option as default to keep my disks sleeping, and disable default labels.

Currently, [zfsonlinux/zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot) is no longer being actively developed. However, it remains a powerful and straightforward shell script tool. Therefore, this fork incorporates several features from other branches.

What has changed in this fork:
- Disable default recursive snapshot strategy. 
- Use localtime by default instead of UTC.
- Subsequent snapshots will not be aborted even if the pre-snapshot command fails once.
- Add `-c` option to detect bytes_written firstly in order to prevent duplicate snapshot and potential disk wake-ups.
- Add `--default-exclude` options to default cron scripts. So only datasets with `com.sun:auto-snapshot=true` will be snapshotted.

```
$ zfs-auto-snapshot -h                                                                
Usage: /usr/local/sbin/zfs-auto-snapshot [options] [-l label] <'//' | name [name...]>
  --default-exclude  Exclude datasets if com.sun:auto-snapshot is unset.
  -c, --changed      Snap only if data written > 0.
  -d, --debug        Print debugging messages.
  -e, --event=EVENT  Set the com.sun:auto-snapshot-desc property to EVENT.
      --fast         Use a faster zfs list invocation.
  -n, --dry-run      Print actions without actually doing anything.
  -s, --skip-scrub   Do not snapshot filesystems in scrubbing pools.
  -h, --help         Print this usage message.
  -k, --keep=NUM     Keep NUM recent snapshots and destroy older snapshots.
  -l, --label=LAB    LAB is usually 'hourly', 'daily', or 'monthly'.
  -p, --prefix=PRE   PRE is 'zfs-auto-snap' by default.
  -q, --quiet        Suppress warnings and notices at the console.
      --send-full=F  Send zfs full backup. Unimplemented.
      --send-incr=F  Send zfs incremental backup. Unimplemented.
      --sep=CHAR     Use CHAR to separate date stamps in snapshot names.
  -g, --syslog       Write messages into the system log.
  -r, --recursive    Snapshot named filesystem and all descendants.
  -v, --verbose      Print info messages.
      --destroy-only Only destroy older snapshots, do not create new ones.
      name           Filesystem and volume names, or '//' for all ZFS datasets.
```

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

```
git clone git@github.com:GrayXu/zfs-auto-snapshot.git
cd zfs-auto-snapshot
make install
zfs set com.sun:auto-snapshot=true tank
```