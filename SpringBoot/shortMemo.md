
- ### 프로젝트 생성
  - [스타터 페이지](https://start.spring.io/) 에서 프로젝트 생성 
  ![setting 화면](../img/jpa_setting.png)
  - application run 후, localhost:8080 접속
  
- ### 롬복 셋팅 
  - Prefrences > plugin > lombok 설치
  - Prefrences > Annotation Processors > Enable annotation processing 체크 (재시작)
  ![img](../img/lombok1.png)
  - 임의의 테스트 클래스를 만들고 @Getter, @Setter 확인
  ![img](../img/lombok2.png)

- ### InteliJ Gradle 대신 자바 직접 실행
  - 기본 설정인 Gradle 로 실행하면 속도가 느림 
  - 자바로 바로 실행해서 실행속도를 높임
  ![img](../img/buildSetting.png)

- ### maven / Gradle 차이
(참고 링크)[https://mylupin.tistory.com/39]