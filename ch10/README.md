# 10부 - 네트워크 모니터링

## 01장 - 기본 방향
## 02장 - 네트워크 기초
L2 스위치 - MAC Address  
L3 스위치 - IP Address  
L4 스위치 - Port Number (특정 IP의 포트번호로 들어오는 패킷을 여러 서버의 특정 포트 번호로 재분배)  
L7 스위치 - Application 내용에 따라 스위칭/로드밸런싱 (HTTP URL에 따른 스위칭/로드밸런싱)  

## 03장 - IP

### 사설 IP 주소
- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

### 사이더(CIDR; Classless Inter-Domain Routing)

### 라우팅 테이블, 라우팅 경로 확인
```
netstat -r
```
`route`도 동일한 기능 수행  

```
traceroute [domain명 | IP 주소]
```
윈도우는 tracert  

## 04장 - TCP
TCP 연결상태 확인
```
netstat -t
```
- -n: 숫자로 표현 (주소 도메인 해석안함)
- -l: Listening 상태만 확인

```shell
$ netstat -t
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0     36 centos7-generic:ssh     192.168.1.1:10137       ESTABLISHED

$ netstat -tn
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0     36 192.168.1.240:22        192.168.1.1:10137       ESTABLISHED

$ netstat -tnl
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:9090            0.0.0.0:*               LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
```

### 네이글(Nagle's) 알고리즘
모아서 보내기


## 05장 - UDP

## 06장 - 기타 프로토콜
### DNS
### FTP

## 07장 - 네트워크 데이터 수집 방법
네트워크 데이터 수집할 때 고려사항  
1. 최대 패킷 크기 명시적 지정  
2. 호스트와 포트 반드시 지정
3. root계정으로 수집 (네트워크 데이터 수집할 때 /dev 아래에 있는 네트워크 디바이스에 접근 - root권한 필요)

### tcpdump
```
tcpdump -i [device] -s [packetsize] -w [output filename] [network filter] -c [count] -C [filesize]
```

```shell
$  tcpdump -i eth1 -s 16000 -w test.dump -c 10
tcpdump: listening on eth1, link-type EN10MB (Ethernet), capture size 16000 bytes
10 packets captured
17 packets received by filter
0 packets dropped by kernel
```

## 08장 - 네트워크 패킷 분석


