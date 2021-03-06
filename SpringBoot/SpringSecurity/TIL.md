### 인증(Authentication) ,  인가(Authorization)
- 인증 : 클라이언트가 요청한 사용자와 같은 사용자인지 확인하는 과정 (= 로그인)
- 인가 : 권한부여, 클라이언트가 하고자 하는 작업이 해당 클라이언트에게 허가된 작업인지 확인
  * 인증 후 => 인증된 사용자에게 특정 권한을 부여(인가)

### 스프링 시큐리티 특징 
- 로그인, 로그아웃 url을 기본적으로 제공하기떄문에 해당 컨트롤러를 만들필요가없다 (별도 지정도 가능)
  * /oauth2/authorization/[소셜 로그인]
  * ex) 구글로그인 /oauth2/authorization/google , 로그아웃 /logout
- 구글은 [구글 클라우드 플랫폼](https://console.cloud.google.com) 에서 client-id, client-secret 값 발급

### 세션 저장소 
1) WAS(톰캣) 세션 사용
- 애플리케이션 배포할때마다 재실행 -> 세션 초기화
- 2대이상의 WAS 환경에서는 세션공유를 위한 추가설정 필요

2) DB를 세션저장소로 사용
- 여러 WAS간의 공용 세션 사용할수 있는 방법
- 로그인 요청마다 DB I/O 가 발생하여 성능이슈 위험
- 요청이 많지않은 백오피스, 사내시스템으로 주로 사용 , 비용 절감

3) Redis, MemCached .. 메모리 DB를 세션저장소로 사용
- 내장형 (Embeded)이 아닌 외부 메모리 서버가 필요
- B2C 서비스에서 가장 많이 사용