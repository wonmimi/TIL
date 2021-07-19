# 아이템 27. 비검사 경고를 제거하라

제네릭에 익숙해질수록 마주치는 경고 수는 줄겠지만 한번에 컴파일 된다고 기대할 수 없다
> 대부분의 비검사 경고는 쉽게 제거될 수 있다
```java
  Set<String> exaltation = new HashSet();
```
```shell
$javac item27.java -Xlint:unchecked
item27.java:8: warning: [unchecked] unchecked conversion
        Set<String> exaltation= new HashSet();
                                ^
  required: Set<String>
  found:    HashSet
```
<img width="60%" alt="uncheck" src="https://user-images.githubusercontent.com/66981136/126149290-c57282f1-e80e-434f-a00b-44c392c203e3.png">

컴파일러에서 알려준대로 수정하면 경고가 사라진다
```java
  Set<String> exaltation = new HashSet<>();
```
비검사 경고를 모두 제거하면 타입 안정성이 보장되어 런타임에 `ClassCastException`이 발생할 일이 없다
  * ClassCastException : 클래스 형 변환(캐스팅) 연산을 잘못 했을 때 발생한 예외

> @SuppressWarning

제거하기 어려운 경고일 경우
- 경고를 제거할수 없지만 안전한 타입이라고 확신하면 `@SuppressWarning("unchecked")` 을 달아 경고를 숨긴다
- 비검사 경고를 그대로 두면, 진짜 문제를 알리는 경고가 파묻혀서 눈치채지 못할 수 있다.

> @SuppressWarning 애너테이션은 가능한 좁은 범위에서 적용하자

보통 변수 , 짧은 메서드 , 생성자 선언
- 클래스 전체에 적용하면 심각한 경고를 놓칠수 있다

<img width="75%" alt="suppresswarning" src="https://user-images.githubusercontent.com/66981136/126157088-ceade6fc-8f67-4d14-b735-779624756322.png">

 한 줄이 넘는 메서드나 생성자에 적용한 애너테이션은 지역 변수 선언 쪽으로 옮긴다
 
```java
public <T> T[] toArray(T[] a) {
        if (a.length < size) {
            @SuppressWarnings("unchecked") 📌
             T[] result = (T[]) Arrays.copyOf(elements, size, a.getClass()); 📌
            return result;
        }
        ...
    }
```
- 애너테이션을 사용할 땐 그 경고를 무시해도 안전한 이유를 항상 주석으로 남긴다 
  - 다른사람이 코드를 잘못 수정하여 타입 안정성을 잃는 상황을 줄여준다

## 정리
- 모든 비검사 경고는 런타임에 ClassCastException을 일으킬 수 있는 잠재적 가능성이 있으므로 최선을 다해 제거하라
- 경고를 없앨 방법을 찾지 못할경우, 가능한 한 범위를 좁혀 @SuppressWarnings 에너테이션으로 경고를 숨기고<br> 그 근거를 주석으로 남겨라




- - - 
[컴파일에러 , 런타임에러](https://codedragon.tistory.com/3509)

[javac --unchecked](https://bgpark.tistory.com/33)

[참고 - 1](https://hirlawldo.tistory.com/79)

[참고 - 2](https://vsfe.tistory.com/45)

[참고 -3 ](https://incheol-jung.gitbook.io/docs/study/effective-java/undefined-3/2020-03-20-effective-27item)

[참고 - 4](https://jjingho.tistory.com/71)