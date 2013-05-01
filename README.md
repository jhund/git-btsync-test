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
