# Docker 명령어 모음

### 이미지 검색하고 pull 받기

```
> docker search [이미지 이름]
```

해당 이미지를 docker hub라는 사이트에서 검색한 결과들을 보여줌

---

### 이미지 다운로드

```
> docker pull nginx

// docker pull nginx:latest
// docker pull nginx:1.21.1
```

pull 명령어를 이용해 다운로드

```
> docker images
```

가지고 있는 이미지들 출력

---

### 컨테이너 만들기

```
> docker run -d --name testserver -p 80:8080 nginx:1.21.1

// docker run -d --name [컨테이너 이름] -p [호스트 포트넘버]:[컨테이너 포트넘버] [이미지 이름]
```

- run : 해당 이미지를 통해 컨테이너를 만들고 구동하는 명령어
- -d : 컨테이너를 만들면 백그라운드에서 곗속 구동하게 하는 옵션. (데몬이라 칭함)
- --name : 컨테이너의 이름을 지정해주는 옵션. (따로 지정해주지 않으면 무작위로 생성됨)
- -p : 호스트의 port, 컨테이너의 port 넘버를 서로 연결해주는 옵션

---

### 실행중인 컨테이너 확인하기

```
> docker ps
```

ps는 process status를 줄인 것

### 모든 컨테이너 확인하기

```
> docker ps -a
```
