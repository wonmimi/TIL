### OOP( ( Object Oriented Programing )
: 객체 지향 프로그래밍

- 추상화, 캡슐화, 정보은닉, 다형성
- 접근지정자 final : 상수선언 할떄 사용(static final). 상속과 변경 금지 
  - 클래스일경우 오버라이딩 X
- 람다식  [참고]('http://yoonbumtae.com/?p=2776')
```java
List<Dog> dogs1 = names.stream()
                .map(x -> new Dog(x)) // 적용 전
                .collect(Collectors.toList());
List<Dog> dogs2 = names.stream()
                .map(Dog::new) // 적용 후
                .collect(Collectors.toList());
```

- 상속 extends, implements [차이](https://velog.io/@hkoo9329/%EC%9E%90%EB%B0%94-extends-implements-%EC%B0%A8%EC%9D%B4)
- - - 
