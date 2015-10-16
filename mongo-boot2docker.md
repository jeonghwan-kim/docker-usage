Boot2docker로 몽고디비 실행시 에러 처리
=================================


## 호스트 폴더 추가시 오류

관련 이슈: [https://github.com/docker-library/mongo/issues/30](https://github.com/docker-library/mongo/issues/30)

컨테이너 생성시 호스트 폴더(/Users/Chris/tmp/data)를 추가하면 아래 로그가 발생한다.

```
docker run -it -v /Users/Chris/tmp/data:/data/db --name mongo mongo:2.4

mongod --help for help and startup options
Fri Oct 16 08:02:12.695 [initandlisten] MongoDB starting : pid=1 port=27017 dbpath=/data/db/ 64-bit host=c581e6c7278f
Fri Oct 16 08:02:12.695 [initandlisten] db version v2.4.14
Fri Oct 16 08:02:12.695 [initandlisten] git version: 05bebf9ab15511a71bfbded684bb226014c0a553
Fri Oct 16 08:02:12.696 [initandlisten] build info: Linux ip-10-154-253-119 2.6.21.7-2.ec2.v1.2.fc8xen #1 SMP Fri Nov 20 17:48:28 EST 2009 x86_64 BOOST_LIB_VERSION=1_49
Fri Oct 16 08:02:12.696 [initandlisten] allocator: tcmalloc
Fri Oct 16 08:02:12.696 [initandlisten] options: {}
Fri Oct 16 08:02:12.699 [initandlisten] Assertion: 13651:Couldn't fsync directory '/data/db': errno:22 Invalid argument
0xdea831 0xdabffb 0xdac53c 0x93265c 0xa020d1 0x6d7fc9 0x6d885d 0x6df570 0x6e1319 0x7f36f7755ead 0x6cfde9
 mongod(_ZN5mongo15printStackTraceERSo+0x21) [0xdea831]
 mongod(_ZN5mongo11msgassertedEiPKc+0x9b) [0xdabffb]
 mongod() [0xdac53c]
 mongod(_ZN5mongo16flushMyDirectoryERKN5boost11filesystem34pathE+0x29c) [0x93265c]
 mongod(_ZN5mongo15acquirePathLockEb+0x1d1) [0xa020d1]
 mongod(_ZN5mongo14_initAndListenEi+0x389) [0x6d7fc9]
 mongod(_ZN5mongo13initAndListenEi+0x1d) [0x6d885d]
 mongod() [0x6df570]
 mongod(main+0x9) [0x6e1319]
 /lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xfd) [0x7f36f7755ead]
 mongod(__gxx_personality_v0+0x499) [0x6cfde9]
Fri Oct 16 08:02:12.702 [initandlisten] exception in initAndListen: 13651 Couldn't fsync directory '/data/db': errno:22 Invalid argument, terminating
Fri Oct 16 08:02:12.703 dbexit:
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: going to close listening sockets...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: going to flush diaglog...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: going to close sockets...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: waiting for fs preallocator...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: lock for final commit...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: final commit...
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: closing all files...
Fri Oct 16 08:02:12.703 [initandlisten] closeAllFiles() finished
Fri Oct 16 08:02:12.703 [initandlisten] shutdown: removing fs lock...
Fri Oct 16 08:02:12.704 dbexit: really exiting now
```

아직 미해결 이슈이다.

> DO NOT try mounting OS X folders into boot2docker hosted Docker containers now

osx의 호스트 폴더를 마운팅하지 말아야한다.


## 볼륨추가시 용량 부족

컨테이너 생성시 아래 로그가 발생한다.

```
docker run -it -v /home/docker/data:/data/db --name mongo mongo:2.4

mongod --help for help and startup options
Fri Oct 16 07:50:09.904 [initandlisten] MongoDB starting : pid=1 port=27017 dbpath=/data/db/ 64-bit host=c309d95b0c05
Fri Oct 16 07:50:09.904 [initandlisten] db version v2.4.14
Fri Oct 16 07:50:09.904 [initandlisten] git version: 05bebf9ab15511a71bfbded684bb226014c0a553
Fri Oct 16 07:50:09.904 [initandlisten] build info: Linux ip-10-154-253-119 2.6.21.7-2.ec2.v1.2.fc8xen #1 SMP Fri Nov 20 17:48:28 EST 2009 x86_64 BOOST_LIB_VERSION=1_49
Fri Oct 16 07:50:09.905 [initandlisten] allocator: tcmalloc
Fri Oct 16 07:50:09.905 [initandlisten] options: {}
Fri Oct 16 07:50:09.905 [initandlisten] journal dir=/data/db/journal
Fri Oct 16 07:50:09.906 [initandlisten] recover : no journal files present, no recovery needed
Fri Oct 16 07:50:09.906 [initandlisten]
Fri Oct 16 07:50:09.907 [initandlisten] ERROR: Insufficient free space for journal files
Fri Oct 16 07:50:09.907 [initandlisten] Please make at least 3379MB available in /data/db/journal or use --smallfiles
Fri Oct 16 07:50:09.907 [initandlisten]
Fri Oct 16 07:50:09.907 [initandlisten] exception in initAndListen: 15926 Insufficient free space for journals, terminating
Fri Oct 16 07:50:09.907 dbexit:
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: going to close listening sockets...
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: going to flush diaglog...
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: going to close sockets...
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: waiting for fs preallocator...
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: lock for final commit...
Fri Oct 16 07:50:09.908 [initandlisten] shutdown: final commit...
Fri Oct 16 07:50:09.909 [initandlisten] shutdown: closing all files...
Fri Oct 16 07:50:09.909 [initandlisten] closeAllFiles() finished
Fri Oct 16 07:50:09.909 [initandlisten] journalCleanup...
Fri Oct 16 07:50:09.909 [initandlisten] removeJournalFiles
Fri Oct 16 07:50:09.909 [initandlisten] shutdown: removing fs lock...
Fri Oct 16 07:50:09.909 dbexit: really exiting now
```

추가할 볼륨의 여유 공간이 3379MB 이상이어야한다.
폴더 용량을 확인해 보니

```
df -h
Filesystem                Size      Used Available Use% Mounted on
tmpfs                     1.8G    106.4M      1.7G   6% /
tmpfs                  1001.2M         0   1001.2M   0% /dev/shm
/dev/sda1                18.2G      2.9G     14.3G  17% /mnt/sda1
cgroup                 1001.2M         0   1001.2M   0% /sys/fs/cgroup
/dev/sda1                18.2G      2.9G     14.3G  17% /mnt/sda1/var/lib/docker/aufs
none                    111.9G     97.3G     14.5G  87% /Users
```

여유 공간이 1.7G다.
14.3G 여유가 있는 /mnt/sda1 폴더를 사용하면 되겠다.

```
docker run -it -v /mnt/sda1/data:/data/db --name mongo mongo:2.4
```
