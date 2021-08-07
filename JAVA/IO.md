## 자바 입출력 I/O 

> ### StringBuilder 
StringBuilder 클래스는 String 클래스와 유사하지만 
- String 클래스는 불변, StringBuilder 클래스는 가변적
  -  String 클래스의 문자열은 한 번 생성되면 메모리 내부에서 변경 불가능하여 문자열 결합.. 등을 할때 메모리 내에서 새로운 문자열이 만들어진다
  - StringBuilder 클래스는 문자열 결합등 연산을 할때 새로운 문자열을 만들지 않고 기존의 문자열에 추가된다

[+](https://hardlearner.tistory.com/288)

#### 메모리, 시간 비교 
동일한 코드에서 String 과 StringBuilder 사용하였을때 비교 

<img width="60%" alt="stringBuilder" src="https://user-images.githubusercontent.com/79403710/127774367-501b37ea-4d01-49e8-8acc-96c1a5541bd2.png">

[-> 코드 ](https://github.com/wonmimi/java-programming-skills/blob/main/src/Algorithm/BOJ/Search/Code03.java)

<!-- [참고](https://ict-nroo.tistory.com/61) -->