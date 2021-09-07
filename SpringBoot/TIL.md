### InteliJ Gradle 대신 자바 직접 실행
  - 기본 설정인 Gradle 로 실행하면 속도가 느림 
  - 자바로 바로 실행해서 실행속도를 높임
  ![img](../img/buildSetting.png)
- - - 
### maven / Gradle 차이
  참고 [링크](https://mylupin.tistory.com/39)
- - - 
### thymeleaf 템플릿 엔진 
  레퍼런스 [링크](https://www.thymeleaf.org/)
- - - 
### 내장 WAS 사용 
  외장 으로 사용하지 않기때문에 서버 변동 (버전 업)이 있을경우에도 무리가 없다.
- - - 
### 정적 파일 위치 
 js, css , image 등의 정적파일들은 src/main/resources/static 하위에 위치 하여 
url에서는 '/'로 설정 
  - ex) src/main/resources/static/image = (http:도메인/image/...) 