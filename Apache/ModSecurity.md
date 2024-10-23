# Apache ModSecurity

- Apache ModSecurity는 웹 애플리케이션 방화벽(WAF, Web Application Firewall)의 역할을 하는 모듈이다.
- 웹 서버에서 들어오는 요청을 실시간으로 분석하고, 보안 규칙에 따라 악성 요청을 차단하거나 기록한다. 
- 주로 웹 애플리케이션을 SQL 인젝션, XSS(크로스 사이트 스크립팅), 파일 포함 공격 등 다양한 공격으로부터 보호하는 데 사용된다.
- Apache 서버에 모듈 형태로 설치되며, Nginx 와 같은 다른 웹 서버에서도 호환 가능한 버전이 있다. 이를 통해 애플리케이션 레이어의 보안을 강화할 수 있다.

<br/>

## 주요 기능
- 웹 요청 및 응답 검사 : HTTP 요청 및 응답을 분석하여 보안 규칙에 위배되는 트래픽을 차단하거나 허용 
- 보안 규칙 엔진 : 정해진 규칙에 따라 다양한 공격 패턴을 탐지하고 대응할 수 있는 기능 제공 
- 로깅 및 감사 : 웹 서버로 들어오는 모든 트래픽에 대한 로그를 남기고, 감사 기록을 생성
- OWASP 규칙 세트 통합 : OWASP Core Rule Set(CRS)와 같은 사전 정의된 규칙 세트를 사용해 일반적인 공격을 차단

<br/>

## 간단한 사용
주로 `/etc/httpd/modsecurity.d/activated_rules/` 디렉터리에 규칙 파일을 추가하거나 수정하여 사용한다.

```bash
# 설치 (이후 httpd 서비스 재시작 필요)
yum install -y mod_security mod_security_crs httpd

# 설치 확인
httpd -M | grep security
```

```bash
# 일반적인 설정 파일
/etc/httpd/conf.d/mod_security.conf
/etc/httpd/modsecurity.d/modsecurity.conf
/etc/httpd/modsecurity.d/activated_rules/
```

<br/>

## 알아본 계기

* 아이폰 웹앱에서 동영상 업로드 시 컨트롤러를 타지 않는 이슈가 생겨 mod_security를 확인하게 되었고, id "200003" 에 대한 문제로 확인하여 SecRuleRemoveById 200003를 추가하여 해결하였다. 
* id 200003 은 _MULTIPART_UNMATCHED_BOUNDARY_ 로, 이 오류는 **멀티파트 요청의 바운더리(boundary)** 가 일치하지 않을 때 발생한다. 다음은 이 오류가 발생하는 주요 이유이다.

  - 잘못된 요청 형식: 클라이언트에서 보낸 멀티파트 요청이 올바른 형식이 아닐 경우
  - 바운더리 문제: HTTP 헤더의 Content-Type에 명시된 바운더리 값과 실제로 전송된 데이터의 바운더리가 일치하지 않을 경우
  - 네트워크 문제: 전송 중 데이터 손실이나 변경이 발생하여 바운더리가 손상

