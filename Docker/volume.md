# Docker Volume

- Docker에서 볼륨(Volume)은 컨테이너와 호스트 시스템 간의 데이터 공유와 지속성을 제공하는 방법이다.

- 볼륨을 사용하면 컨테이너가 재시작되거나 삭제되더라도 데이터가 유지된다. 컨테이너가 데이터를 영구적으로 저장해야 하는 경우에 유용하게 사용 가능하다.


### - Docker 볼륨의 특징
- 데이터 지속성 : 컨테이너가 삭제되거나 재시작되어도 볼륨에 저장된 데이터는 유지된다.
- 데이터 공유 : 여러 컨테이너가 동일한 볼륨을 사용할 수 있다.
- 호스트 독립성 : 호스트 시스템의 파일 시스템에 영향을 미치지 않으므로, 데이터의 이동과 관리가 더 간편하다.

---

### 볼륨 생성

```bash
> docker volume create my_volume
```
<br/>

### 볼륨 사용
컨테이너를 실행할 때 `-v` 옵션을 사용하여 볼륨을 마운트할 수 있다.

```bash
# `my_volume`이라는 볼륨을 컨테이너의 `/path/in/container` 디렉토리에 마운트

> docker run -d --name my_container -v my_volume:/path/in/container my_image

```
<br/>

### 볼륨 확인
```bash
> docker volume ls
```
<br/>

### 볼륨 상세 정보 확인

```bash
> docker volume inspect my_volume
```
<br/>

### 볼륨 삭제
```bash
> docker volume rm my_volume
```
<br/>

---

<br/>

## - 예제: MySQL 컨테이너와 볼륨
MySQL 컨테이너를 실행하면서 데이터베이스 데이터를 영구적으로 저장하기 위해 볼륨을 사용

1. 볼륨 생성:

    ```bash
    > docker volume create mysql_data
    ```

2. MySQL 컨테이너 실행:

    ```bash
    > docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -v mysql_data:/var/lib/mysql mysql:5.7
    ```

위 명령어는 MySQL 데이터 디렉토리(`/var/lib/mysql`) 를 `mysql_data` 볼륨에 마운트. <br/>
컨테이너가 삭제되더라도 데이터는 `mysql_data` 볼륨에 남아 있다.

<br/>

## - 예제: 호스트 디렉토리와 볼륨
`-v` 옵션으로 호스트의 디렉토리를 컨테이너 디렉토리에 마운트 시킴

```bash
# 호스트의 `/host/path` 디렉토리를 컨테이너의 `/container/path` 디렉토리에 마운트

> docker run -d --name my_container -v /host/path:/container/path my_image
```
<br/>

## - 예제: 자동 볼륨 생성
```bash
> docker run -d --name my_container -v /container/path my_image
```
위처럼 호스트 디렉토리를 지정하지 않고 컨테이너의 /path/in/container에 볼륨을 마운트하게 되면 <br/>
Docker는 자동으로 익명 볼륨을 생성하여 컨테이너에 마운트한다.

