### REST api HTTP method 타입
- 생성 (Create) : POST
- 읽기 (Read) : GET
- 수정 (Update) : PUT , PATCH
- 삭제 (Delete) : DELETE
- put / patch 의 차이 
  * put : 전달한 필드외 모두 나머지 필드는 null 혹은 default 값으로 처리 (전체 데이터 수정에 용이)
  * patch : 전달한 필드 데이터만 수정 , 나머지 필드 유지  (일부 데이터 수정에 용이)
[참고](https://devuna.tistory.com/77)