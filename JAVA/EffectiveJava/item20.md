# 아이템 20. 추상 클래스 보다는 인터페이스를 우선하라

> java가 제공하는 2가지 다중 구현 방식

- 추상클래스 : 추상클래스가 정의한 타입을 구현하는 클래스는 반드시 추상클래스의 하위클래스가 되어야 한다
  * 자바는 단일상속만 지원하여 새로운 타입을 정의하는데 큰 제약을 받는다
- 인터페이스 : 선언한 메서드를 모두 정의한 클래스라면 다른 어떤 클래스를 상속하든 상관없이 같은 타입으로 취급 한다

> 인터페이스의 장점 
###  기존 클래스에도 손쉽게 새로운 인터페이스를 구현해 넣을 수 있다
인터페이스에서 요구하는 메서드를 추가하고 implements 구문만 추가 하면 된다

### 믹스인(mixin)정의에 적합하다
믹스인이란 어떤 클래스의 주된 기능에 선택적 기능을 `혼합`한 것이다.
  * 예시로 `Comparable`은 자신을 구현한 클래스의 인스턴스끼리 순서를 정할 수 있다고(compareTo) 선언하는 믹스인 인터페이스다.

_하지만_ 추상 클래스는 기존 클래스가 만약 다른 클래스를 상속하고 있다면 추가적인 기능을 혼합할 수 없으므로 믹스인에 적합하지 않다

### 계층구조가 없는 Type 프레임워크를 만들 수 있다
계층을 엄격히 구분하기 어려운 개념일 때 유용하다
```java
public interface Singer { AudioClip sing(Song s); } // 가수

public interface SongWriter { Song compose(int chartPosition); } // 작곡가

public interface SingerSongWriter { // 가수 + 작곡가
    AudioClip sing(Song s);
    void actSensitive(); // 📌 제3의 메서드 추가
}
```

### 추상 골격 구현 클래스
자바8 부터 인터페이스에 [`디폴트 메서드`](https://frontierdev.tistory.com/67)가 지원되어 메서드를 구현 할수 있게 되었지만 제약이 있다
 -  Object 메서드인 equals와 hashcode를 디폴트 메서드로 제공할 수 없다
 - 인스턴스 필드를 가질 수 없고 public이 아닌 정적 멤버를 가질 수 없음.
 - 본인이 만든 인터페이스가 아니면 디폴트 메서드를 추가할 수 없음.

추상 골격 구현 클래스를 제공하여 인터페이스와 추상 클래스의 장점 모두 취할 수 있다.
- 인터페이스로 타입을 정의하고 메서드 구현이 필요한 부분은 추상 골격 구현 클래스에서 구현한다 ( `템플릿 메서드 패턴`)
- 필요하면 디폴트 메서드도 함께 제공한다.
```java
public interface Character {
    public void wakeup();
    public void gohome();
    public void work();
    public void process();
}

public abstract class AbstractCharacter implements Character{  📌// 관례상 Abstract를 접두어로 사용
    @Override
    public void wakeup() {
        System.out.println("일어나서 출근한다");
    }

    @Override
    public void gohome() {
        System.out.println("퇴근한다\n");
    }

    @Override
    public void process() { 📌 // 템플릿 메서드
        wakeup();
        work();
        gohome();
    }
}

public class Teacher extends AbstractCharacter implements Character{
    @Override
    public void work() {
        System.out.println("수업을 한다");
    }
}

public class Police extends AbstractCharacter implements Character{
    @Override
    public void work() {
        System.out.println("순찰을 한다");
    }
}

public static void main(String[] args) {
    Teacher teacher = new Teacher();
    Police police = new Police();
    teacher.process();
    police.process();
}

```
-  중복된 부분을 추상 골격 구현 클래스(AbstractCharacter)로 정의하여 불필요한 중복을 없앨 수 있다


>  일반적으로 다중 구현 타입으로 재사용성, 유연성 그리고 다형성 측면에서 인터페이스를 우선하는 것이 옳다 <br>
더불어 복잡한 인터페이스라면 구현하는 수고를 덜어주는 골격 구현을 꼭 고려해보자


---
[참고 1](https://it-mesung.tistory.com/192)

[참고 2](https://insight-bgh.tistory.com/405)

[참고 3](https://yhmane.tistory.com/182#recentComments)

[참고 3](https://icarus8050.tistory.com/77?category=419017)

[디폴트 메서드](https://frontierdev.tistory.com/67)