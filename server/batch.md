## Batch Job

- 대규모 데이터를 처리하거나 반복적인 작업을 자동화하기 위해 일정한 시점에 실행되는 작업

- 웹사이트와 사용자의 브라우저 사이의 통신이 암호화 -> 데이터가 제3자에 의해 가로채지거나 변조되지 않도록 보호
- SSL (Secure Socket Layer : 데이터 전송 암호화 프로토콜) 인증서 필요
  - 정보를 안전하게 전송하기 위한 디지털 보증서, 데이터 전송 중 변경되지 않았음을 보장
  - SSL 인증서 발급 위해서는 Domain이 필요 (SSL 인증서는 웹사이트의 Domain과 연결되어 있음)
  - SSL 인증서 설정은 NginX 또는 SpringBoot 내장서버인 Tomcat에서 가능하지만 Reverse Proxy 서버가 처리하는게 일반적


    |NginX에 설정|SpringBoot에 설정|
    |------|---|
    |- SSL 암ㆍ복호화 작업을 NginX가 수행하므로 백엔드 서버는 암호화 작업 X <br/>&nbsp;&nbsp;&nbsp;-> 성능 향상<br/>- 인증서가 NginX에 중앙집중화<br/>&nbsp;&nbsp;&nbsp;-> 인증서 관리 용이 |- NginX가 필요 없음<br/>- 암ㆍ복호화 작업을 SpringBoot 서버가 수행<br/>&nbsp;&nbsp;&nbsp;-> cpu 자원 많이 소모<br/>- 여러 서버 운영시 인증서 관리 어려움 |
   

#### - SSL 인증서 발급 방법
```
  Let's Encrypt : 무료로 SSL/TLS 인증서를 발급해주는 인증 기관(Certificate Authority)
  Certbot : Let's Encrypt에서 제공하는 인증서를 발급받고 설치하는데 사용하는 도구. 인증서 발급 및 갱신 과정이 자동화  
  인증서는 90일 동안 유효하고 30일 남으면 자동 갱신
  -> sudo systemctl list-timers하면 certbot.timer 등록되어 있음
```
- ubuntu 서버에 Certbot 설치
- sudo apt install certbot python3-certbot-nginx
- sudo certbot --nginx -d example.com -d www .example.com (도메인 주소에 대한 인증서 발급, 설치)
- sudo certbot renew --dry-run (자동 갱신 작동 확인)

- sudo certbot ceretificates (인증서 정보 확인) 
