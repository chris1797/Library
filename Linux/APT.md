# APT (Advanced Package Tool) 란?

- APT는 Debian 및 Debian 기반 리눅스 배포판(Ubuntu 등)에서 소프트웨어 패키지를 관리하기 위한 시스템이다.

- 소프트웨어 패키지의 설치, 업그레이드, 제거를 쉽게 수행할 수 있게 해준다. 

- `apt` 명령어는 APT의 주요 명령어를 단순화한 명령어로, 일반 사용자와 시스템 관리자 모두에게 편리한 인터페이스를 제공한다.

<br/>

## APT의 주요 명령어

- `apt update`: 패키지 목록을 업데이트
- `apt upgrade`: 설치된 패키지를 최신 버전으로 업그레이드
- `apt install <package_name>`: 지정된 패키지를 설치
- `apt remove <package_name>`: 지정된 패키지를 제거
- `apt search <package_name>`: 패키지 목록에서 지정된 패키지를 검색
- `apt show <package_name>`: 지정된 패키지에 대한 정보를 표시

<br/>

## APT의 작동 방식

#### 1. `apt update`

`apt update` 명령어는 패키지 목록을 업데이트하고 이 과정에서 `/etc/apt/sources.list` 파일과 `/etc/apt/sources.list.d/` 디렉토리에 정의된 저장소 목록에서 최신 패키지 목록을 가져온다. <br/>

`apt update` 명령어는 패키지를 설치하거나 업그레이드하기 전에 항상 실행하는 것이 좋다.

- **작동 방식:**
  1. APT는 소스 리스트 파일(`/etc/apt/sources.list`)과 추가 소스 파일(`/etc/apt/sources.list.d/`)을 읽음
  2. 각 저장소에 대해 HTTP(S) 요청을 보내어 패키지 인덱스 파일을 다운로드
  3. 다운로드된 패키지 인덱스 파일을 `/var/lib/apt/lists/` 디렉토리에 저장
  4. 로컬 시스템에 저장된 패키지 목록을 최신 상태로 갱신


<br/>

#### 2. `apt install <package_name>`

`apt install <package_name>` 명령어는 지정된 패키지를 설치한다.

- **작동 방식:**
  1. APT는 로컬 패키지 목록을 검색하여 설치할 패키지와 해당 패키지의 의존성을 확인
  2. 의존성 트리를 계산하여 필요한 모든 패키지를 결정
  3. 필요한 패키지를 저장소에서 다운로드
  4. `dpkg`를 사용하여 다운로드된 패키지를 설치
  5. 설치 중에 필요하면 설정 스크립트를 실행하여 패키지를 구성


<br/>

## 요약

- APT는 Debian 기반 시스템에서 패키지 관리를 단순화하고 자동화하는 도구이다.
- `apt update`는 패키지 목록을 최신 상태로 갱신하여 최신 버전의 패키지를 참조할 수 있게 한다.
- `apt install`은 패키지를 설치하며 의존성 문제를 자동으로 해결한다.
- 위 두 명령어를 적절히 사용하여 시스템을 최신 상태로 유지하고 필요한 소프트웨어를 쉽게 설치할 수 있다.
