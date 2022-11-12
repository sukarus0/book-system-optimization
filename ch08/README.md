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

