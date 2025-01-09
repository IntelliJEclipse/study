## Domain
특정 웹사이트나 리소스를 찾을 때 사용하는 식별자

|도메인 종류|설명|예|
|------|---|---|
|최상위 도메인 (TLD, Top-Level Domain) |도메인 이름의 끝부분 (일반/국가)|.com, .org, .net, .kr|
|2차 도메인 (주 도메인)|최상위 도메인 앞에 위치하는 부분 (기업/서비스 이름)|google, naver, example|
|서브 도메인 |주 도메인의 앞에 추가적인 식별자로 사용 (기능 구별)|blog.example.com에서 blog|

#### - DNS (Domain Name System) - 도메인 이름을 특정 IP주소로 변환
#### - Domain 등록 방법

- Domain 등록 대행 업체 선택 (Google Domains, Amazon Route 53z,  Azure DNS 등, 국내 업체도 있음 - 현재 mailplug에서 도메인 구매하여 사용 중)
- Domain 이름 선택하여 등록, 개인 정보 입력 후 결제
- Domain 설정 및 관리 
  - DNS RECORD : DNS에서 사용하는 정보
  - A record (Address Record) : 도메인 이름을 ip 주소에 mapping
  - CNAME (Canonical Name Record) : 서브 도메인에 포워딩, www.~ 주소를 리디렉션
  - MX (record mail exchange record) :  이메일을 처리할 메일 서버의 주소를 설정
  - TXT record (Text Record) : 도메인에 대한 텍스트 정보 저장, 주로 인증과 보안 위해 사용 (도메인 소유권 인 위해 Google, Microsoft가 사용)
  - NS record (Name Server Record) : 도메인의 네임서버를 지정

#### - 브라우저는 도메인을 ip 주소로 변환 할때 일정 시간동안 ip 주소를 캐시, 이 캐시는 TTL(Time-To-Live) 값에 의해 관리됨
 * TTL(Time-To-Live)
 
   - 해당 IP 주소가 캐시에 저장될 수 있는 최대 기간
   - IP 주소를 캐시에 저장해두고, 같은 도메인에 접근할 때 로컬 캐시를 사용
   - TTL이 만료 시 DNS 서버에 다시 요청
   

