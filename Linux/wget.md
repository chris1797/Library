# wget 명령어

- `wget`은 리눅스 및 유닉스 계열 운영 체제에서 주로 사용되는 명령어로, 웹에서 파일을 다운로드할 때 사용 <br/>

- `wget`은 GNU 프로젝트의 일부로 개발된 자유 소프트웨어이며 HTTP, HTTPS, FTP 프로토콜을 지원

<br/>

1. **재귀적 다운로드**: `-r` 옵션을 사용하여 웹사이트 전체를 다운로드할 수 있다.
2. **중단된 다운로드 재개**: 중단된 다운로드를 이어받을 수 있는 기능을 지원한다.
3. **백그라운드 다운로드**: 다운로드를 백그라운드에서 실행할 수 있다.
4. **대량 다운로드**: 여러 파일을 한 번에 다운로드할 수 있다.
5. **프록시 지원**: 프록시 서버를 통해 다운로드할 수 있다.

<br/>

### 사용 예시:

```bash
wget http://example.com/file.zip
```

위 명령어는 `http://example.com/file.zip` 파일을 현재 디렉토리로 다운로드하는 것.

<br/>

중단된 다운로드를 재개하고자 할 때는 `-c` 옵션을 사용할 수 있다:

```bash
wget -c http://example.com/largefile.zip
```

## 주요 옵션
- -V, --version: wget의 버전을 출력합니다.
- -h, --help: 사용 가능한 옵션들을 보여주는 도움말을 출력합니다.
- -c, --continue: 이전에 중단된 다운로드를 이어서 진행합니다.
- -P, --directory-prefix=PREFIX: 다운로드한 파일을 저장할 디렉토리를 지정합니다.
- -r, --recursive: 웹사이트를 재귀적으로 다운로드합니다.
- -l, --level=NUMBER: 재귀적 다운로드의 깊이를 지정합니다.
- -k, --convert-links: 다운로드한 파일 내의 링크들을 로컬 링크로 변환합니다.
- -m, --mirror: 사이트를 미러링합니다. (-r, -N, -l inf, -nr 옵션을 포함)
- -q, --quiet: 진행 상황을 출력하지 않습니다.
- --limit-rate=RATE: 다운로드 속도를 제한합니다.
- -b, --background: 백그라운드에서 실행합니다.
- --user=USER: HTTP 서버에 접속할 사용자 이름을 지정합니다.
- --password=PASS: HTTP 서버에 접속할 비밀번호를 지정합니다.
- -U, --user-agent=AGENT: 사용자 에이전트를 설정합니다.
- --no-check-certificate: SSL 인증서를 검사하지 않습니다.
