# Docker 명령어 모음

### 이미지 검색하고 pull 받기

```bash
$ docker search [이미지 이름]
```

해당 이미지를 docker hub라는 사이트에서 검색한 결과들을 보여줌

---

### 이미지 다운로드

```bash
> docker pull nginx

// docker pull nginx:latest
// docker pull nginx:1.21.1
```

pull 명령어를 이용해 다운로드

```bash
> docker images
```

가지고 있는 이미지들 출력

---

### 컨테이너 만들기

```bash
> docker run -d --name testserver -p 80:8080 nginx:1.21.1

// docker run -d --name [컨테이너 이름] -p [호스트 포트넘버]:[컨테이너 포트넘버] [이미지 이름]
```

- run : 해당 이미지를 통해 컨테이너를 만들고 구동하는 명령어
- -d : 컨테이너를 만들면 백그라운드에서 곗속 구동하게 하는 옵션. (데몬이라 칭함)
- --name : 컨테이너의 이름을 지정해주는 옵션. (따로 지정해주지 않으면 무작위로 생성됨)
- -p : 호스트의 port, 컨테이너의 port 넘버를 서로 연결해주는 옵션

---

### ⭐️ 실행중인 모든 컨테이너 확인하기

```bash
> docker ps
```

ps는 process status를 줄인 것

### 모든 컨테이너 확인하기

```bash
> docker ps -a
```

</br>

# 컨테이너를 생성 후 실행 (run)

```bash
> docker run -it centos:latest bash
```
docker run -it <이미지이름:태그> <명령어> 로 컨테이너 실행</br>
일반적으로 이미지를 기반으로 새로운 컨테이너를 생성하고 시작하는 데 사용됨
이때 SSH를 통해 서버에 접속한 것이 아니라, 호스트OS와 격리된 환경에서 bash 프로그램을 실행한 것</br>

### run 옵션

-it : 대화형으로 컨테이너 실행</br>
-p : 호스트와 컨테이너 간의 포트 매핑, 호스트포트:컨테이너포트</br>
-d : 백그라운드에서 실행

</br>

# 실행 중인 컨테이너에 추가 명령어 실행 (exec)

```bash
> docker exec -it [컨테이너 이름 or ID] /bin/bash
```

</br>

# 실행 중이지 않은 컨테이너 실행 (start)
```bash
> docker start [컨테이너 이름 or ID] /bin/bash
```
