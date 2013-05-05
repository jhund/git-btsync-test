git-btsync-test
===============

This repo is for testing how git and Bittorrent Sync (btsync) can be used in 
parallel to synchronize WIP on two local dev machines.

btsync is Bittorrent's P2P dropbox replacement. I'm really excited about this 
because:

* I can sync directly from peer to peer without going through a middle man.
* My local harddrive storage and network bandwidth is the limit at no 
  additional cost.
* I can sync as many folders as I want, each with its own unique configuration.


This test is important to me since I would like to be able to switch seamlessly
between my desktop and my laptop. With seamless I mean:

I'm working on a Rails app and I have some uncommitted local changes. I have
sat for 2 hours at my desk, and I'd like to do some work at my standing workstation
on my laptop. I just save everything, stand up, walk over to the standing workstation,
where all my changes are already waiting for me. I just continue working on 
a different machine in an identical environment. Sweet.


With this test I'm trying to answer these questions:
----------------------------------------------------

* Will btsync and git step on each others' toes?
* Will btsync synch the .git folder?
* Will git be surprised when all the changes after a pull are already present in 
  the local files?

Test setup
----------

1. iMac with OS X 10.6 (Snow Leopard)
2. Macbook Pro with OS X 10.8 (Mountain Lion)

Current versions of git and btsync on both machines, and this repo as a test repo.

What I found out
----------------

1. btsync syncs the .git directory. So both the source code and the git meta 
   info will be sync'd.
2. When syncing a git repository root folder, I added `.Sync*` to `.gitignore`. 
   I want these files to be managed by btsync only.
3. I confirmed the significant sync delay on OS X 10.6. I observed about 10 minutes.
   This is a documented bug:
   http://forum.bittorrent.com/topic/16410-bittorrent-sync-faq/
   >  (due to system peculiarities, sync on Mac OS X 10.6 may be delayed up to 10 minutes)
   Btsync is fairly fast when I make the change on OS X 10.8 (< 30 seconds).
4. git can get out of sync temporarily in the following scenario:
    1. I make changes on computer A, and btsync updates them immediately on
       computer B. At this point both computers have uncommitted changes.
    2. I commit the changes on computer A and push them to origin.
    3. Here is where the race condition occurs: When I immediately try to pull
       on computer B, git complains that I have uncommitted changes.
    4. If I wait a little bit, the git meta files will have been sync'd as well
       and now the changes are not uncommitted any more, the working tree is
       clean.
    I think this issue was exacerbated by the sync delays on OS X 10.6.
    It was all cleaned up once the .git directory was sync'd.
5. When doing rails dev, I add the following files to `.SyncIgnore`:
   `log/*` and `tmp/*`.

Conclusion
----------

It's looking very promising so far. I have been going back and forth between the
two computers, not even thinking about where I made a change.
