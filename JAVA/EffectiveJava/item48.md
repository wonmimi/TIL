# 아이템 48. 스트림 병렬화는 주의해서 적용하라

동시성 프로그래밍 측면에서 자바는 항상 앞서갔다
- 자바 8부터 `parallel`메서드만 한 번 호출하면 파이프라인을 병렬 실행할 수 있는 스트림을 지원하기 시작

동시성 프로그래밍을 할 때는 `안전성(Safety)`과 `응답가능(liveness)` 상태를 유지해야 하는데 병렬 스트림 파이프라인 프로그래밍에서도 마찬가지

하지만, 환경이 아무리 좋더라도 
  - 데이터 소스가 [`Stream.iterate()`](https://www.daleseo.com/java9-stream-iterate/) 이거나,
  - 중간 연산으로 `limit()`를 사용하는 경우엔 <br>

  __파이프라인 병렬화의 성능 개선을 기대할 수 없다__
  ```java
    public static void main(String[] args) {
      // 메르센 소수 생성 (item 45)
        primes().map(p -> TWO.pow(p.intValueExact()).subtract(ONE))
                .filter(mersenne -> mersenne.isProbablePrime(50))
                .limit(20)
                .forEach(mp -> System.out.println(mp.bitLength() + ": " + mp + "  "));
    }

    static Stream<BigInteger> primes() {
        return Stream.iterate(TWO, BigInteger::nextProbablePrime);
    }
  ```

  ```java
    // parallel() 메소드 적용 📌
    primes().parallel().map(p -> TWO.pow(p.intValueExact()).subtract(ONE))
              ...
            .forEach(mp -> ...);
  ```

> ## 병렬화 효율이 좋은 자료구조
스트림의 소스가 다음과 같을때 병렬화 효과가 좋다
- ArrayList, HashMap, HashSet, [ConcurrentHashMap](https://devlog-wjdrbs96.tistory.com/269)의 인스턴스 
- 배열 , int, long 범위

위 자료구조들은 데이터를 원하는 크기로 나눌 수 있어 다수의 스레드에 분배하기에 좋고,<br> 
`참조 지역성(locality of reference)` 이 높아 성능이 좋다
  * 참조 지역성 : 이웃원소의 참조들이 메모리에 연속해서 저장되어 있다
  * `기본 타입 배열`은 (참조가 아닌) 데이터 자체가 메모리에 연속되어 저장되므로 참조지역성이 가장 뛰어난 자료구조이다

참조 지역성이 낮으면 스레드가 주 메모리에서 캐시 메모리로의 전송을 기다리며 대부분 시간을 낭비 하게 된다 <br>
따라서 벌크 연산을 병렬화할 때 `참조 지역성`은 중요한 요소로 작용한다

> ## 스트림 파이프라인의 종단 연산
스트림 파이프라인의 종단연산의 동작방식은 병렬 수행 효율에 영향을 준다
- 병렬화에 가장 적합한 종단 연산은 `축소(reduction)`이다.
  * 축소: 파이프라인에서 만들어진 모든 원소를 하나로 합치는 작업
- `min, max, sum, count` 같이 완성된 형태로 제공되는 메서드, 
-  `xxxMatch`처럼 (anyMatch, allMatch, noneMatch) 조건에 맞으면 바로 반환되는 메서드가 병렬화에 적합

반면, Stream의 `collect` 메서드는 컬렉션들을 합치는 부담이 크기 때문에 병렬화에 적합하지 않다 (가변 축소)

 > ## 스트림 파이프라인의 명세 규약
스트림을 잘못 병렬화하면 성능이 나빠질 뿐만 아니라 결과 자체가 잘못되거나 예상 못한 동작이 발생할 수 있다 (안전 실패)
  - <h6>안전 실패(safety failure) : 병렬화한 파이프라인이 사용하는 mappers, filters 혹은 프로그래머가 제공한 다른 함수 객체가 명시한대로 동작하지 않을 때 발생</h6>

Stream 명세는 이때 사용되는 함수 객체에 관한 규약을 정의
- reduce 연산에 건네지는 accumulator(누적기)와 combiner(결합기) 함수는 반드시 결합법칙을 만족해야 한다
    * 결합 법칙 : (a op b) op c = a op (b op c)
- 간섭받지 않아야 한다 (non-interfering)
  * 파이프라인이 수행되는 동안 데이터소스가 변경되지 않아야한다
- 상태를 갖지 않아야 한다 (stateless)

위의 요구사항을 지키지 못하더라도 순차적으로 실행하면 올바른 결과를 얻을 수 있다

> ## 스트림 병렬화는 오직 성능 최적화 수단
- 다른 최적화와 마찬가지로 변경 전후로 반드시 `성능테스트`를 진행하여 병렬화를 사용할 가치가 있는지 확인해야 한다(아이템 67)
  * 잘못된 파이프라인 하나가 다른 부분의 성능에까지 악영향을 줄 수 있음을 유의
- 조건이 잘 갖춰지면 `parallel` 메서드 호출 하나로 프로세서 코어 갯수에 비례하는 성능으로 향상할 수 있다
  * 머신러닝, 데이터 처리 분야

<h6>스트림 파이프라인 병렬화 효과</h6>

```java
// 소수 계산 스트림 파이프라인
  static long pi(long n) {
      return LongStream.rangeClosed(2, n)
              .mapToObj(BigInteger::valueOf)
              .filter(i -> i.isProbablePrime(50))
              .count();
  }

  public static void main(String[] args) {
      long start = System.currentTimeMillis();
      System.out.println(pi(10_000_000));
      long end = System.currentTimeMillis();
      System.out.println("time : " + (end - start));
  }
```
병렬화 하지 않을경우 ≒ 20s

<img width="50%" alt="" src="https://user-images.githubusercontent.com/66981136/129599105-043aed0d-ff9e-49b8-8acc-611c49ee0d58.png">

```java
static long pi(long n) {
      return LongStream.rangeClosed(2, n)
              .parallel() 📌
              .mapToObj(BigInteger::valueOf)
              .filter(i -> i.isProbablePrime(50))
              .count();
  }
```
병렬화 ≒ 4s 

<img width="50%" alt="" src="https://user-images.githubusercontent.com/66981136/129598883-7fb2ba85-8a72-44fc-9018-fc7242834252.png">

## 정리 
> - 스트림 병렬화를 잘못하면 프로그램이 오동작하거나 성능이 급격히 떨어질 수 있다
> - 병렬화를 할 경우에는 성능테스트를 반드시 진행하고, 결과가 정확한지 확인해야 한다
> - 계산도 정확하고 성능도 좋아짐이 확실한 경우에 병렬화 버전을 운영 코드에 반영하여야 한다



---
### ref
- Effectiva Java 3판
<!-- - [ref 1](https://jaehun2841.github.io/2019/02/17/effective-java-item48/)
- [ref 2](https://incheol-jung.gitbook.io/docs/study/effective-java/undefined-5/48) -->