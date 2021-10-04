# 아이템 88. readObject메서드는 방어적으로 작성하라

[아이템 50](https://github.com/chosun-dev/study-book-effective-java/blob/dev/8%EC%9E%A5/50_%EC%A0%81%EC%8B%9C%EC%97%90_%EB%B0%A9%EC%96%B4%EC%A0%81_%EB%B3%B5%EC%82%AC%EB%B3%B8%EC%9D%84%20_%EB%A7%8C%EB%93%A4%EB%9D%BC_%EC%A0%95%EC%9B%90%EB%AF%B8.md) 에서 가변인 Date 필드의 불변을 유지하기 위해 생성자와 접근자에서 Date객체를 방어적 복사본을 만들어 사용하였다
```java
public Period(Date start, Date end) {
    this.start = new Date(start.getTime());
    this.end   = new Date(end.getTime());
    ...
}
```
Priod 클래스를 `기본 직렬화`를 사용할 경우 불변식을 보장하지 못한다
  - `implements Serializable`을 추가


### readObject 메서드
__매개변수로 바이트 스트림을 받는 생성자__ <br>

 ❗️ __불변을 깨뜨릴 의도__ 로 만들어진 바이트 스트림을 매개변수로 받으면 정상적인 방법으로는 만들어낼 수 없는 객체를 생성할 수 있다

 - Ex) 
    ```java
    // 정상적인 Period 인스턴스를 직렬화한 후에 의도적으로 수정한 바이트 스트림
        private static final byte[] serializedForm = {
                (byte)0xac, (byte)0xed, 0x00, 0x05, 0x73, 0x72, 0x00, 0x06,
                0x50, 0x65, 0x72, 0x69, 0x6f, 0x64, 0x40, 0x7e, (byte)0xf8,
                0x2b, 0x4f, 0x46, (byte) 0xc0, (byte) 0xf4, 0x02, 0x00, 0x02,
                ... 생략 ...
                
        };

        public static void main(String[] args) {
            Period p = (Period) deserialize(serializedForm);
            System.out.println(p);
        }
    ```
    결과
    ```
    Fri Jan 01 12:00:00 PST 1999 - Sun Jan 01 12:00:00 PST 1984
    ```

  `readObject` 메서드가 실질적으로 또다른 public 생성자이기 때문에 생성자와 똑같은 수준으로 주의를 기울여야한다

## 불변식을 만족하는 방법
### 유효성 검사
`readObject` 메서드가 `defaultReadObject`를 호출하게 한 후에 역직렬화된 객체가 유효한지 검사해야 한다. 
 - 유효성 검사에 실패한다면 `InvalidObjectException`을 던져 잘못된 역직렬화가 발생하는 것을 막는다

- ex) Period 클래스
  ```java
  private void readObject(ObjectInputStream s) throws IOException, ClassNotFoundException {
      s.defaultReadObject();
      
      // 불변식 만족 검사
      if(start.compareTo(end) > 0) { 📌
        throw new InvalidObjectException(start + "가 " + end + "보다 늦다.");
      }

      ...
  }
  ```

### private 가변요소를 방어 복사
Period 인스턴스에서 시작된 바이트 스트림 끝에 Date 필드의 참조를 추가하여 __가변 Period 인스턴스를 만들어낼 수 있다__
  - 공격자가 역직렬화를 통해 바이트 스트림 끝의 참조 값을 읽으면 Period의 Date정보를 얻을 수 있다
  - 이 참조를 이용하여 Date를 수정할 수 있게 되어 불변식이 꺠진다

```java
private void readObject(ObjectInputStream s) throws IOException, ClassNotFoundException {
    s.defaultReadObject();
    
    // 가변 요소들의 방어적 복사본 생성
    start = new Date(start.getTime());  📌
    end = new Date(end.getTime());  📌
    
    if(start.compareTo(end) > 0) { 
      throw new InvalidObjectException(start + "가 " + end + "보다 늦다.");
    }

    ...
}
```
- 객체를 역직렬화할 때는 클라이언트가 제어하면 안되는 객체 참조 필드를 모두 반드시 방어적으로 복사해야 한다
  * readObject에서는 불변 클래스 안의 모든 private 가변 요소를 방어적으로 복사 해야한다
- `final` 필드는 방어적 복사가 불가능하니 이를 제거해서 사용해야 한다

## 기본 readObject 사용
- `transient` 필드를 제외한 모든 필드의 값을 매개변수로 받아 유효성 검사 없이 public 생성자에 추가해도 괜찮다 ?
  * O : 기본 readObject 메서드 사용
  * X : 커스텀 readObject 메서드를 만들어 유효성 검사와 방어적 복사 수행
- `직렬화 프록시 패턴`(아이템 90)을 사용해도 된다 (권장)
  * 역직렬화를 안전하게 만드는 데 필요한 노력을 줄여준다

## 정리
> - 안전한 readObject 메서드 작성 지침
>   *  private 이여야 하는 객체 참조 필드는 각 필드가 가리키는 객체를 방어적으로 복사하라
>   * 방어적 복사 다음에는 반드시 불변식 검사가 뒤따르고, <br>
      검사에 어긋나는 게 발견되면 InvalidObjectException을 던진다
>   * 재정의(Overriding) 가능한 메서드는 호출하지 말자
>      - 하위 클래스의 상태가 완전히 역직렬화 되기전에 하위 클래스에서 재정의된 메서드가 실행되어
프로그램 오작동을 일으킬 수 있다


---
### Ref
- [readObject](https://madplay.github.io/post/what-is-readobject-method-and-writeobject-method)
