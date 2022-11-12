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
