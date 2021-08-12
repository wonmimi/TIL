# 아이템 31. 한정적 와일드카드를 사용해 API 유연성을 높이라

제네릭에서 매개변수화 타입은 `불공변 (invariant)`이다 <br>
즉, 서로 다른 두 타입이 있을 때 하위 타입도 상위 타입도 아니므로 부모-자식 관계가 성립되지 않는다


#### 불공변 특성보다 유연한 설계가 필요할 때는 한정적 와일드카드를 이용한다
> 생산자(Producer) 매개변수에 와일드카드 적용

```java
public void pushAll(Iterable<E> src) {  // 결함이 있다
        for (E e : src)
            push(e);
}
```
스택에 원소를 추가하려고 할때

<img width="70%" alt="item31" src="https://user-images.githubusercontent.com/66981136/127865759-b7cabc7c-0a43-4338-9163-7540d8273dd9.png">

Integer는 Number의 하위타입이지만 매개변수 타입은 __불공변__ 이기 때문에 컴파일시 에러가 난다 (타입이 일치하면 문제 없다)

 이런 경우 매개변수화 타입에 한정적 `와일드카드 타입`을 사용하여 유연하게 안전한 타입으로 만들 수 있다
  * 생산자 파라미터 : 메서드 내 컬렉션으로 원소를 옮기는 입력 매개변수
  <!-- 입력 매개변수로부터 메서드 내 컬렉션으로 원소를 옮긴다 -->
 ```java
 public void pushAll(Iterable<? extends E> src) { 📌 
      for (E e : src)
          push(e);
  }
 ```
  `<? extends E>` : 자기 자신 포함 E의 하위타입이 파라미터여야 한다는 의미

<br>


> 소비자(Consumer) 매개변수에 와일드카드 적용

원소를 꺼내고 싶을 경우 
```java
public void popAll(Collection<E> dst) {   // 결함이 있다
        while (!isEmpty())
            dst.add(pop());
}
```
pushAll과 마찬가지로 타입 불공변으로 컴파일 에러

<img width="70%" alt="item31" src="https://user-images.githubusercontent.com/66981136/127878375-0d14ef4e-5989-4c2d-8ade-3f1eecd92285.png">

 소비자 파라미터에 `super`를 이용한 `와일드카드 타입`을 사용하여 해결한다
  * 소비자 파라미터 : 메서드의 컬렉션 인스턴스의 원소를 옮겨 담은 입력 매개변수
  <!-- 메서드의 컬렉션 인스턴스의 원소를 입력 매개변수로 옮겨 담는다 -->
```java
public void popAll(Collection<? super E> dst) { 📌
        while (!isEmpty())
            dst.add(pop());
}
``` 
 `<? super E>` : 자기 자신포함 E의 상위타입이 파라미터여야 한다는 의미

<!-- - __모든 타입은 자기 자신의 상,하위타입__ 이므로 두가지 모두 자기 자신의 타입도 가능하다 -->
- __유연성을 극대화 하려면 원소의 생산자나 소비자용 입력매개변수에 와일드카드 타입을 사용하라__

<br>

> 펙스(PECS) : Producer-Extends, Consumer-Super
<!-- 입력 매개변수가 생산자,소비자 역할을 동시에 한다면 타입을 정확히 지정해야 하기 때문에 와일드카드를 쓸수 없다 -->
PECS 공식은 와일드카드 타입을 사용하는 기본 원칙

 - 매개변수화 타입 T가 생산자라면 <? extends T>를 사용하고, 소비자라면 <? super T>를 사용하면 된다
    * pushAll의 매개변수는 Stack이 사용할 E 인스턴스를 생산하는 `생산자`
    * popAll의 매개변수는 Stack으로부터 E 인스턴스를 소비하는 `소비자`

#### PECS 규칙 적용 
```java
public static <E extends Comparable<E>> E max(List<E> list){
  ...
  for (E e : list)
      if (result == null || e.compareTo(result) > 0)
      result = Objects.requireNonNull(e);
  ...
}
```
PECS 규칙을 적용하여 수정할 경우
```java
public static <E extends Comparable<? super E>> E max(List<? extends E> list) { ... } 
```
- 입력 매개변수는 E 인스턴스를 생산하므로 <? extends E> 
- 타입 매개변수는 `Comparable은 언제나 소비자`이므로 일반적으로 Comparable<? super E>를 사용하는 편이 더 낫다. (`Comparator`도 마찬가지)

#### 타입 매개변수와 와일드카드 둘 중 어느 것을 사용해도 괜찮을 때
<!-- 타입 매개변수와 와일드카드에는 서로 공통되는 부분이 있어서 메서드를 정의할 때 둘중 아무거나 사용해도 괜찮다 -->

리스트 내의 특정 인자들의 위치를 바꾸는 경우,
```java
  public <E> swap(List<E> list, int i, int j){ .. } 1️⃣
  public void swap(List<?> list, int i, int j){ 2️⃣ 📌
    list.set(i, list.set(j, list.get(i)));
  }
```
public API 라면 __와일드카드를 사용한 두번째 메소드가 낫다__ <br>
어떤 리스트든 이 메서드에 넘기면 원소들을 교환하고 신경써야할 타입 매개변수도 없다

- 메서드 선언에 타입 매개변수가 한 번만 나오면 와일드카드로 대체한다
   * 비한정적 타입 매개변수(아이템 30) 라면 비한정적 와일드카드로 바꾸고
   * 한정적 타입 매개변수라면 한정적 와일드 카드로 바꾼다

<br>

리스트의 타입이 한정적 와일드카드인 List<?>은 nul 이외에는 어떤 값도 넣을 수 없어서 와일드카드 타입의 실제 타입을 알려주는 private 도우미 메서드를 따로 이용한다.
```java
public static void swap(List<?> list, int i, int j) {
    swapHelper(list, i, j);
}

private static <E> void swapHelper(List<E> list, int i, int j) {
    list.set(i, list.set(j, list.get(i)));
}
``` 
 메서드 내부에서는 더 복잡한 제네릭 메서드를 이용했지만 메서드를 호출하는 클라이언트는 와일드카드 기반의 깔끔한 메서드를 유지할 수 있다
 

## 정리 
  > - 복잡하더라도 와일드카드 타입을 적용하여 훨씬 유연한 API를  작성한다
  > - 생산자(producer)는 extends, 소비자(consumer)는 super 인 PECS 공식을 기억하자
  > - Comparable과 Comparator는 모두 소비자라는 사실도 잊지 말자
  


- - - 
### ref 
- [참고 1](https://jaehun2841.github.io/2019/01/26/effective-java-item31/#%EC%84%9C%EB%A1%A0)
- [참고 -2](https://hirlawldo.tistory.com/83)
- [참고 3](https://jaehun2841.github.io/2019/01/26/effective-java-item31/#%EC%8B%AC%ED%99%942-%EC%99%80%EC%9D%BC%EB%93%9C%EC%B9%B4%EB%93%9C%EB%A5%BC-%EC%A0%81%EC%A0%88%ED%9E%88-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
- [참고 4](https://icarus8050.tistory.com/86)
- [참고 5](https://incheol-jung.gitbook.io/docs/study/effective-java/undefined-3/2020-03-20-effective-31item)