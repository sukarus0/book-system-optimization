# 08부 - 서버 OS 모니터링

## 01장 - 기본 방향
CPU > 메모리 > 디스트 > 네트워크  

## 02장 - CPU

### CPU 사용량 상세 분석 
- System : 커널 모드 시스템 콜에 사용된 CPU 사용량 (Windows에서는 Privileged)
- User : 시스템 콜 이외의 일반 함수 실행에 사용된 CPU 사용량
- IO Wait : CPU를 할당받아 사용할 수 있는 상태가 되었으나, 입출력을 기다리고 있어 CPU 사용할 수 없는 상태의 사용량 (실제 CPU사용량은 아님)
- Idle : 사용하지 않는 CPU 여유율

`sar -u 1`
```shell
$ sar -u 1
Linux 3.10.0-1160.76.1.el7.x86_64 (centos7-generic)     11/12/2022      _x86_64_        (4 CPU)

03:17:47 PM     CPU     %user     %nice   %system   %iowait    %steal     %idle
03:17:48 PM     all      0.25      0.00      0.49      0.00      0.00     99.26
03:17:49 PM     all      0.73      0.00      0.97      0.00      0.00     98.30
03:17:50 PM     all      0.50      0.00      0.99      0.00      0.00     98.51
^C

03:17:51 PM     all      0.60      0.00      1.19      0.00      0.00     98.21
Average:        all      0.50      0.00      0.86      0.00      0.00     98.63
```

### 동시 다중 스레딩 (SMT; Simultaneous Multi-Threading)
기존 싱글 쓰레드에서는 프로세스 사이클마다 1개의 명령을 실행  
-> SMT에서는 프로세스 사이클마다 다수의 명령을 실행  
Intel CPU에서는 `하이퍼 쓰레딩`이라고 함  

### perf


## 03장 - 메모리
- 파일 캐시
- IPC
- 프로세스 메모리

### vmstat
```shell
$ vmstat 3 5
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0      0 6370980   2116 921236    0    0     3     4   57   40  0  0 99  0  0
 0  0      0 6371756   2116 921236    0    0     0     0  359  226  0  0 99  0  0
 0  0      0 6370520   2116 921236    0    0     0     0  450  251  0  1 99  0  0
 0  0      0 6370708   2116 921236    0    0     0     0  351  235  0  1 99  0  0
 0  0      0 6369760   2116 921236    0    0     0     5  437  266  0  1 99  0  0
```

### 파일 캐시 메모리
```shell
$ free -m
              total        used        free      shared  buff/cache   available
Mem:           7820         696        6222          16         901        6810
Swap:          2047           0        2047
```

### IPC 메모리
```shell
$ ipcs -a

------ Message Queues --------
key        msqid      owner      perms      used-bytes   messages

------ Shared Memory Segments --------
key        shmid      owner      perms      bytes      nattch     status

------ Semaphore Arrays --------
key        semid      owner      perms      nsems
```

### 프로세스 메모리
```shell
$ ps -auf
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root     23343  0.0  0.0 116740  3312 pts/1    Ss   03:24   0:00 -bash
root     23429  0.0  0.0  20044  1152 pts/1    S+   03:24   0:00  \_ tmux a -t 0
root     19032  0.0  0.0 116844  3424 pts/6    Ss   03:18   0:00 -bash
root     21351  0.0  0.0 157348  2204 pts/6    S+   03:21   0:10  \_ watch -n 5 ping -c 2 www.google.com
root     14789  0.0  0.0 157344   568 pts/6    S+   15:37   0:00      \_ watch -n 5 ping -c 2 www.google.com
root     14790  0.0  0.0 132756  1716 pts/6    S+   15:37   0:00          \_ ping -c 2 www.google.com
```

## 04장 - 디스크

### 디스크 소요시간
```
sar -d [second] [count]
```
```shell
$ sar -d 3 2
Linux 3.10.0-1160.76.1.el7.x86_64 (centos7-generic)     11/12/2022      _x86_64_        (4 CPU)

03:40:17 PM       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
03:40:20 PM    dev8-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:40:20 PM  dev253-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:40:20 PM  dev253-1      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00

03:40:20 PM       DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
03:40:23 PM    dev8-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:40:23 PM  dev253-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
03:40:23 PM  dev253-1      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00

Average:          DEV       tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz     await     svctm     %util
Average:       dev8-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
Average:     dev253-0      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
Average:     dev253-1      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```

### 디스크 입출력 모니터링
```
iostat [option] [second] [count]
```
```shell
$ iostat -x 3 2
Linux 3.10.0-1160.76.1.el7.x86_64 (centos7-generic)     11/12/2022      _x86_64_        (4 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.22    0.00    0.38    0.00    0.00   99.39

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.00     0.01    0.60    0.15    11.47    14.31    68.95     0.00    1.45    0.39    5.68   0.46   0.03
dm-0              0.00     0.00    0.57    0.16    11.32    14.36    69.60     0.00    1.54    0.40    5.51   0.47   0.03
dm-1              0.00     0.00    0.00    0.00     0.03     0.00    50.09     0.00    0.31    0.31    0.00   0.24   0.00

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.42    0.00    0.67    0.00    0.00   98.92

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.00     0.00    0.33    0.00     2.66     0.00    16.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-0              0.00     0.00    0.33    0.00     2.66     0.00    16.00     0.00    0.00    0.00    0.00   0.00   0.00
dm-1              0.00     0.00    0.00    0.00     0.00     0.00     0.00     0.00    0.00    0.00    0.00   0.00   0.0
```

### 파일 입출력 발생량과 용도
- 프로세스별 사용 파일 목록: lsof, /proc/[pid]/fd
- 프로세스별 입출력 모니터링: strace
- 프로세스 입출력 위치 확인: pstack, jstack

```shell
$ lsof | head -10
COMMAND     PID   TID    USER   FD      TYPE             DEVICE  SIZE/OFF       NODE NAME
systemd       1          root  cwd       DIR              253,0       224         64 /
systemd       1          root  rtd       DIR              253,0       224         64 /
systemd       1          root  txt       REG              253,0   1632960  101033208 /usr/lib/systemd/systemd
systemd       1          root  mem       REG              253,0     20064       8458 /usr/lib64/libuuid.so.1.3.0
systemd       1          root  mem       REG              253,0    265576     240203 /usr/lib64/libblkid.so.1.1.0
systemd       1          root  mem       REG              253,0     90160        593 /usr/lib64/libz.so.1.2.7
systemd       1          root  mem       REG              253,0    157440        595 /usr/lib64/liblzma.so.5.2.2
systemd       1          root  mem       REG              253,0     23968       8669 /usr/lib64/libcap-ng.so.0.0.0
systemd       1          root  mem       REG              253,0     19896       8659 /usr/lib64/libattr.so.1.1.0
```

### 디스크 관리
LVM (Logical Volume Manager)  
파일시스템 - 논리 볼륨 - 볼륨 그룹 - 물리 볼륨  
  
- 파일시스템: df
- 논리볼륨 목록: lvdisplay
- 볼륨그룹 목록: vgdisplay, vgscan
- 물리볼륨 목록: pvdisplay
- 볼류그룹 정보: vgdisplay -v

```shell
$ lvdisplay
  --- Logical volume ---
  LV Path                /dev/centos_centos7/swap
  LV Name                swap
  VG Name                centos_centos7
  LV UUID                cTjDR4-a0Yc-1qz4-ZhVF-mDq3-ICnj-kSEldM
  LV Write Access        read/write
  LV Creation host, time localhost, 2022-08-25 18:58:51 +0000
  LV Status              available
  # open                 2
  LV Size                2.00 GiB
  Current LE             512
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/centos_centos7/root
  LV Name                root
  VG Name                centos_centos7
  LV UUID                ycG2vs-G28h-471e-d7mh-I1At-q3O4-7CknOG
  LV Write Access        read/write
  LV Creation host, time localhost, 2022-08-25 18:58:52 +0000
  LV Status              available
  # open                 1
  LV Size                <125.00 GiB
  Current LE             31999
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0
```

```shell
$ vgdisplay
  --- Volume group ---
  VG Name               centos_centos7
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <127.00 GiB
  PE Size               4.00 MiB
  Total PE              32511
  Alloc PE / Size       32511 / <127.00 GiB
  Free  PE / Size       0 / 0
  VG UUID               uBtbx7-ZGuP-IO2d-LWLi-wU0C-NA0h-8ngFmq
```

```shell
$ pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos_centos7
  PV Size               <127.00 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              32511
  Free PE               0
  Allocated PE          32511
  PV UUID               8rkATn-0QCn-7Z0q-O2Ds-VDyc-EuSe-Dg1dWN
```

한꺼번에 보기  
```shell
$ vgdisplay -v
  --- Volume group ---
  VG Name               centos_centos7
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <127.00 GiB
  PE Size               4.00 MiB
  Total PE              32511
  Alloc PE / Size       32511 / <127.00 GiB
  Free  PE / Size       0 / 0
  VG UUID               uBtbx7-ZGuP-IO2d-LWLi-wU0C-NA0h-8ngFmq

  --- Logical volume ---
  LV Path                /dev/centos_centos7/swap
  LV Name                swap
  VG Name                centos_centos7
  LV UUID                cTjDR4-a0Yc-1qz4-ZhVF-mDq3-ICnj-kSEldM
  LV Write Access        read/write
  LV Creation host, time localhost, 2022-08-25 18:58:51 +0000
  LV Status              available
  # open                 2
  LV Size                2.00 GiB
  Current LE             512
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/centos_centos7/root
  LV Name                root
  VG Name                centos_centos7
  LV UUID                ycG2vs-G28h-471e-d7mh-I1At-q3O4-7CknOG
  LV Write Access        read/write
  LV Creation host, time localhost, 2022-08-25 18:58:52 +0000
  LV Status              available
  # open                 1
  LV Size                <125.00 GiB
  Current LE             31999
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0

  --- Physical volumes ---
  PV Name               /dev/sda2
  PV UUID               8rkATn-0QCn-7Z0q-O2Ds-VDyc-EuSe-Dg1dWN
  PV Status             allocatable
  Total PE / Free PE    32511 / 0
  ```


### 스토리지
- DAS(Directed Attached Storage): 애플리케이션 <- 파일시스템 -> 디스크 스토리지
- NAS(Network Attached Storage): 애플리케이션 <- 네트워크 --- 파일시스템 -> 디스크 스토리지
- SAN(Storage Area Network): 애플리케이션 <- 파일시스템 --- 네트워크 -> 디스크 스토리지

`LUN (Logica Unit Number)`: SCSI 버스에서 사용되는 고유 식별자  


## 레이드 (RAID)
- 레이드 0 : 스트라이핑
- 레이드 1 : 미러링
- 레이드 0+1 : 스트라이핑과 미러링
- 레이드 4 : 스트라이핑과 전용 패리티
- 레이드 5 : 스트라이핑과 분산 패리티


## 05장 - 통합 모니터링 도구

### nmon
```
# 실시간 모니터링 모드 명령
nmon -s [second] -b
s: 갱신간격
b: 블랙 or 화이트

# 기록 모드 명령
nmon -f -s [second] -c [count] -m [directory] -[모니터링 항목 상세옵션]
```

### top
