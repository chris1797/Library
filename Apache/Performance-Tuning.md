# Apache Server Performance Tuning

```bash
<IfModule mpm_event_module> # 멀티 프로세싱 모듈
    StartServers 4  # 아파치 서버가 시작될 때 생성할 자식 프로세스의 수
    ServerLimit 32  # 서버가 생성할 수 있는 자식 프로세스의 최대 수 
    MinSpareThreads 40  # 최소 유휴 스레드 수
    MaxSpareThreads 120 
    ThreadsPerChild 32  # 각 자식 프로세스가 생성할 스레드 수
    MaxRequestWorkers 1024  # 동시에 처리할 수 있는 최대 요청 수
    MaxConnectionsPerChild 0  # 각 자식 프로세스가 종료되기 전에 처리할 최대 연결 수, 0으로 설정하면 자식 프로세스가 무한히 연결을 처리함
</IfModule>
```

- 유휴 스레드 : 현재 요청을 처리하지 않고 대기 중인 스레드
- MaxRequestWorkers 계산 방법 : MaxRequestWorkers = ServerLimit * ThreadsPerChild
  
