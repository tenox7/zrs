# ZRS - ZFS Rolling Snapshot
This tool emulates rolling snapshots of NetApp on ZFS. I wish ZFS on FreeBSD has such
functionality built in but this wasnt the case. I also wish such tool already existed
but this wasnt the case either. Yes, there are some but I didn't like their bloat.

## Usage
* Place `zrs.sh` in a directory accessible via PATH for cron.
* Add crontab(5) entries for root. You can come up with any schedule you like.
* The syntax is: `zrs.sh <volume> <type> 0 <#max>`
* Volume is name of ZFS volume
* Type is the schedule name like hourly, daily, weekly
* The 3rd parameter must be always 0
* The last parameter is last snapshot sequence. 
* Example: `zrs.sh myvol hourly 0 7` will produce 8 snapshots named hourly0 to hourly7

## Example crontab(5) with 4 types 4 snapshot each
    0 * * * * zrs myvol hourly 0 3
    0 0 * * * zrs myvol daily 0 3
    0 0 * * 0 zrs myvol weekly 0 3
    0 0 1 * * zrs myvol monthly 0 3

# Some people like to keep it for a very long time
    0 * * * * zrs myvol hourly 0 47
    0 0 * * * zrs myvol daily 0 60
    0 0 * * 0 zrs myvol weekly 0 11
    0 0 1 * * zrs myvol monthly 0 24

Note that `/etc/crontab` has a different format than crontab(5)

ZRS is in Public Domain