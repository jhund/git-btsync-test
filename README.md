git-btsync-test
===============

Testing how git and btsync can be used in parallel to synchronize WIP on two local dev machines.

btsync is Bittorrent's Sync app (the P2P dropbox replacement).

Trying to answer these questions:
---------------------------------

* Will they step on each others' toes?
* Will btsync synch .git?
* Will git be surprised when all the change after a pull are already present in the local files?

What I found out:
-----------------

1.) BTsync syncs the .git directory. So both the source code and the git meta info will be sync'd.
2.) Added `.Sync*` to `.gitignore`. I want these files to be managed by btsync.
3.) There seems to be a sync delay on OS X. Not sure what side this is on,
    SnowLeopard or MountainLion... It's fairly fast when I make the change on
    MountainLion (< 30 seconds), however it can take up to 10 minutes when
    I make a change on SnowLeopard.
    Turns out that this is a documented bug. From
    http://forum.bittorrent.com/topic/16410-bittorrent-sync-faq/
    >  (due to system peculiarities, sync on Mac OS X 10.6 may be delayed up to 10 minutes)
