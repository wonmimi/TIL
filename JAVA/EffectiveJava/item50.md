# 아이템 50. 적시에 방어적 복사본을 만들라

> ## 자바는 안전한 언어다
[JVM](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0) 이 메모리를 관리하기때문에 시스템의 다른 부분에서 불변식을 지킬 수 있다<br>
__하지만__  클라이언트가 _불변을 깨트릴 경우_ 를 예방하기 위해 방어적인 프로그래밍을 해야 한다
  - 의도적으로 시스템의 보안을 뚫을 경우
  - 프로그래머의 설계 실수

```java
import java.util.Date;

public final class Period {
    private final Date start; // 시작 시간
    private final Date end; // 종료 시간

    public Period(Date start, Date end) {
      if (start.compareTo(end) > 0)
          throw new IllegalArgumentException( // 시작 시간 > 종료 시간일경우 예외처리
                  start + " > " + end);

      this.start = start;
      this.end = end;
        
    }

    //접근자 (getter)
    public Date getStart() {
        return start;
    }
    public Date getEnd() {
        return end;
    }
    ...
}
```
현재 시간을 기준으로 하여 불변으로 여기지만, Date 클래스는 가변객체 이므로 불변성이 깨진다.
```java
  Date start = new Date();
  Date end = new Date();
  Period p = new Period(start, end);
  end.setYear(93);  🟡 // 내부 수정
```
<img width="80%" alt="" src="https://user-images.githubusercontent.com/79403710/130834014-96773a17-3c06-4763-9c9b-d8190318f7ad.png">

 - java 8 이후로는 Date 클래스는 더이상 사용하지 않는다 (낡은 API)
    * Date 대신 불변인 `Instant` ( 또는 `LocalDateTime`, `ZonedDateTime` ) 클래스를 사용

    *  <img width="70%" alt="" src="https://user-images.githubusercontent.com/79403710/130814044-c02bd5d3-9040-4907-ad75-ca286467cc6a.png">

여전히 많은 API 와 내부구현엔 예전에 작성된 낡은 코드들이 많이 남아있다

>## 방어적 프로그래밍 
  _외부로부터 인스턴스의 내부를 보호 하기위함_

### 1) 매개변수를 방어적으로 복사 (defensive copy)
- 생성자에서 받은 가변 매개변수를 복사하여 인스턴스 안에는 복사본을 사용한다
```java
public Period(Date start, Date end) {
    this.start = new Date(start.getTime()); 📌  1️⃣
    this.end   = new Date(end.getTime()); 📌 

    if (this.start.compareTo(this.end) > 0)  2️⃣
          throw new IllegalArgumentException(
              this.start + " > " + this.end);
}
```
- 매개변수의 유효성 검사 전에 방어적 복사본을 만들고 (1️⃣) 복사본으로 유효성을 검사 (2️⃣) 하는 순서로 불변성 보장
  *  찰나에 다른 스레드가 원본객체를 수정할 위험이 있기 때문 → `TOCTOU 공격` (time-of-check/time-of-use)

  * <img width="80%" alt="" src="https://user-images.githubusercontent.com/79403710/130834019-40b8b367-2931-49e9-8952-5a8070812177.png">

- 매개변수가 확장될 수 있는 타입이면 방어적 복사본을 만들 때 clone을 사용해선 안된다
  *  Date는 final 클래스가 아니므로 악의를 가진 하위 클래스의 인스턴스를 재정의하여 반환할 수 있다


### 2) 접근자의 방어적 복사본 반환
__아직__ 접근자(getter) 메서드를 통해 반환한 내부의 가변 정보 Date에 공격이 가능하다 
```java
  start = new Date();
  end = new Date();
  p = new Period(start, end);
  p.getEnd().setYear(95); 🟡 // getter를 통해 내부 수정
  System.out.println(p);
```
- <img width="80%" alt="" src="https://user-images.githubusercontent.com/79403710/130841276-bb534c01-62f7-4c02-9d13-4cfd83556c93.png">

접근자가 가변필드의 방어적 복사본을 반환해 준다
```java
public Date getStart() {
    return new Date(start.getTime()); 📌
}

public Date getEnd() {
    return new Date(end.getTime()); 📌
}
```
- <img width="80%" alt="" src="https://user-images.githubusercontent.com/79403710/130841287-3e52689f-a1d0-4298-9082-c476cc4fcaa8.png">
- 불변식을 위배할 방법은 더이상 존재하지 않게된다 ( [네이티브 메서드](https://minhyeokism.tistory.com/27) , [리플레션](https://dublin-java.tistory.com/53) 제외)
<!-- - 생성자에서와 다르게 접근자 메서드에서는 clone 메서드를 사용해도 된다 
  * Date 객체가 반환될 것임이 확실하기 때문 -->


## 정리 
> - 클라이언트로부터 받거나 클라이언트로 반환하는 구성요소가 가변이라면 클래스의 요소는 반드시 방어적으로 복사해야 한다
> - 복사 비용이 너무 크거나 클라이언트에서 내부를 잘못 수정할 일이 없음을 신뢰한다면 방어적 복사를 생략할 수 있다
> - 대신 해당 요소를 수정했을 때의 책임이 클라이언트에 있음을 문서에 확실히 명시하도록 한다

---
### Ref
- Effectiva Java 3판
<!-- - https://jjingho.tistory.com/99
- https://icarus8050.tistory.com/100
- https://madplay.github.io/post/make-defensive-copies-when-needed -->




