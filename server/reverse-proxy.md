## Reverse Proxy

- 클라이언트의 요청을 대신 받아 다른 서버로 전달하는 서버
- NginX , Apache 등이 있음
- 클라이언트와 실제 애플리케이션서버(springboot 애플리케이션에 내장된 Tomcat 서버) 사이에서 요청과 응답을 중개
- 로드밸런싱, 캐싱 제공, SSL/TLS 암호화처리


#### ubuntu 서버에 Reverse proxy (NginX) 설치
- sudo apt update (패키지 목록 업데이트, apt : Advanced Package Tool)
- sudo apt install nginx (NginX 설치)
- sudo systemctl start nginx
- sudo systemctl enable nginx (시스템 부팅시 자동으로 시작되도록 설정)
- sudo systemctl status (nginx 상태 확인)

#### NginX 설정
- sudo nano /etc/nginx/sites-available/default (nano 편집기를 열어서 설정 파일을 편집)
- 서버 요청 받을 포트, 연결할 도메인 이름, 원본 요청 정보를 백엔드에 전달할 proxy 설정
