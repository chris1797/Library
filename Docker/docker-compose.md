# Docker-compose 명령어 모음

## Docker Compose?

여러 개의 컨테이너로 구성된 애플리케이션을 관리하기 위한 간단한 오케스트레이션 도구. </br>
기본적으로 커맨드가 실행하는 디렉토리에 있는 docker-compose.yml 또는 docker-compose.yaml 파일을 설정 파일로 사용한다.

# -f 옵션

다른 이름이나 경로의 파일을 Docker Compose 설정 파일로 사용하고 싶을 때 -f 옵션을 사용한다. </br>
여러 개의 파일을 사용할 수도 있으며 뒤에 나오는 설정이 우선된다.

```bash
$ docker-compose -f docker-compose-local.yml up

$ docker-compose -f docker-compose.yml -f docker-compose-local.yml up
```

# up ⭐️⭐️⭐️

Docker Compose에 정의되어 있는 모든 서비스 컨테이너를 한번에 생성하고 실행한다. </br>
보통 -d 옵션으로 백그라운드에서 컨테이너를 띄우는데, -d 옵션을 주지 않으면 실행했던 터미널에 컨테이너 로그가 출력되며 빠져나오는 순간 컨테이너들이 모두 정지된다.

```bash
$ docker-compose up -d
```

</br>

## docker-compose.yml 예시
```yml
version: '3'
services:
  web:
    image: httpd:2.4
    volumes:
      - ./apache2/:/usr/local/apache2/ # 설정파일 경로
      - ../upload/:/var/www/html/upload/ #이미지경로
      - C:\workspace\Projects\projectName\projectName_VUE\pages:/var/www/html/projectName
      # 내 PC 경로 : 아파치 서버 내부 경로(linux)
    ports:
      # Listen 정의
      - 8800:8800
      - 8091:8091 #이미지경로
      - 7777:7070
      # 내 PC 포트 : 아파치 포트 
    restart: always

  mysql-local:
    image: mysql:5.7
    container_name: mysql-local
    ports:
      - 3307:3306
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 
    command:
      # 명령어 실행
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_unicode_ci
    volumes:
      - ./cnf/my.cnf:/etc/my.cnf:ro,Z
      - ./data/:/var/lib/mysql # -v 옵션 (다렉토리 마운트 설정)
```
