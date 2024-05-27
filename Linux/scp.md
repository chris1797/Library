# scp 명령어 사용법 (SecureCopy)

## scp란?
- ssh 원격 접속 프로토콜을 기반으로 한 SecureCopy(scp)의 약자. <br/>
- 원격지간, 원격지와 로컬간 디렉토리나 파일을 주고 받을 때 사용하는 파일 전송 프로토콜. <br/>
- ssh와 동일한 22번 포트와 identity 파일을 사용해 송수신하기 때문에 보안적으로도 안정됨.

<br/>

### 로컬 → 원격지
1. 단일 파일을 원격지로 보낼 때

```bash

# scp [옵션][파일명] [원격지_id]@[원격지_ip]:[받는 위치]
> scp test.txt root@192.168.0.27:/tmp/test

```

2. 복수 파일을 원격지로 보낼 때

```bash
# scp [옵션][파일명1][파일명2] [원격지_id]@[원격지_ip]:[받는 위치]
> scp test1.txt test2.txt root@192.168.0.27:/tmp/test
```

3. 디렉토리를 원격지로 보낼 때 (-r 옵션 사용)

```bash
# scp [옵션][디렉토리 이름] [원격지_id]@[원격지_ip]:[받는 위치]
> scp -r testDir root@192.168.0.27:/tmp/test
```

## Options
| option | description |    example    |
|:------:|:-----------:|:-------------:|
|    r   |디렉토리 내 모든 파일 or 디렉토리 복사|     scp -r    |
|    p   |원본 권한 속성 유지 복사|     scp -p    |
| P      |             | scp -P [port] |
| c      |             | scp -c        |
| v      |             | scp -v        |
| a      |             | scp -a        |
