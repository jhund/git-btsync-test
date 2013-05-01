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
4.) git can get out of sync temporarily when I make changes on computer A, commit
    then file updates are sync'd to computer b. Working tree is dirty because
    of local (sync'd) file changes. git pull doesn't work, says to commit
    changes first. However this clears up once all the .git files are sync'd
    and then working tree is clean again. So just give it some time.
    This might have been worse for me since I made the changes on the
    SnowLeopard computer that has an up to 10 minute sync delay.
