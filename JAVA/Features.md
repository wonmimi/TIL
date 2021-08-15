
## 람다식
자바 8부터 함수형 프로그래밍 방식을 지원하고 이를 `람다식`이라 함
- 함수의 구현과 호출만으로 프로그래밍이 수행되는 방식
  문법
  - 익명 함수 만들기
  - 매개 변수와 매개변수를 이용한 실행문 `(매개변수) -> {실행문;}`
  ```java
  (int x, int y) -> {return x+y;}
  ```
  - 매개 변수가 하나인 경우 자료형과 괄호 생략가능
  ```java
    str->{System.out.println(str);}
  ```
  - 실행문이 한 문장인 경우 중괄호 생략 가능
  ```java
    str-> System.out.println(str);
  ```
  - 실행문이 한 문장의 반환문인 경우엔 return과 중괄호를 모두 생략
  ```java
  (x, y) -> x+y;
  str -> str.length;
  ```

## 스트림 (Stream)
배열, 컬렉션을 대상으로 연산을 수행
- 자료에 대한 스트림을 생성하여 연산을 수행하면 스트림은 소모
- 스트림 연산은 기존 자료를 변경하지 않음

메소드
- `map()` :  요소들을 특정조건에 해당하는 값으로 변환
- `filter()`는 요소들을 조건에 따라 걸러내는 작업 
- `sorted()`는 요소들을 정렬해주는 작업
```java
  sList.stream().filter(s->s.length() >= 5).forEach(s->System.out.println(s));

  list.stream().map(s->s.toUpperCase());
```

