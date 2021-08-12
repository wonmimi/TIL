# 아이템 36. 비트필드 대신 EnumSet을 사용하라

> ### 비트필드
열거한 값들이 주로 집합으로 사용 될 경우, 각 상수에 서로 다른 2의 거듭제곱 값을 할당한 정수 열거 패턴( [아이템 34](./34_int_상수_대신_열거_타입을_사용하라_전아름.md) )을 사용해왔다.

```java
// 비트필드 열거 상수
public class Text {
    public static final int STYLE_A = 1 << 0; //1
    public static final int STYLE_B = 1 << 1; //2
    public static final int STYLE_C = 1 << 2; //4
    public static final int STYLE_D = 1 << 3; //8
    

    public void applyStyle(int styles) {...}
}
```
비트별 OR연산을 사용해 여러 상수를 하나로 모아 만든 집합을 `비트 필드(bit field)`라고 한다
```java
  text.applyStyles( STYLE_A | STYLE_C ); // 1 | 2  = 3
```
정수 열거 상수의 단점을 포함한 문제점들을 갖고 있다
  - 비트필드 값을 그대로 출력하면 정수 열거 상수보다 해석하기 어렵다.
    * 어떤 값이 OR연산되서 나온값인지 알기 어렵다
  - 최대 몇 비트가 필요한지 API작성 시 미리 예측이 필요하다 (보통 int, long )
    * API를 수정하지 않고는 비트 수를 수정할 수 없다


> ### EnumSet 클래스
비트 필드의 대안으로 `java.util`패키지의 `EnumSet`클래스를 사용한다 
  - 열거타입 상수의 값으로 구성된 집합을 효과적으로 표현해준다
```java
public class Text {
    public enum Style {A, B, C, D} 📌

    public void applyStyles(Set<Style> styles) { 📌
      ...
    }

```
- Set 인터페이스를 완벽히 구현하여 다른 set 구현체와도 사용 가능하고, 타입 안정성을 갖는다
- 내부는 비트 벡터로 구현되어 있어 원소가 64개 이하라면 long 변수 하나로 표현
- 비트 필드 성능에 버금갈 뿐만 아니라 비트를 직접 다룰 때 발생하는 오류들에서 자유롭다

## 정리
> - 열거할 수 있는 타입을 한데 모아 집합 형태로 사용한다고 해도 비트 필드를 사용할 이유는 없다
> - EnumSet 클래스가 비트 필드 수준의 명료함과 성능까지 제공한다.
> - 유일한 단점은 아직 불변 객체를 생성할 수 없는 점이다.



--- 

### ref 
- [시프트 연산자](https://vmpo.tistory.com/106)
- [참고 ](https://xlffm3.github.io/java/item36-enum-set/)