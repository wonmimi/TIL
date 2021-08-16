# 아이템 48. 스트림 병렬화는 주의해서 적용하라

동시성 프로그래밍 측면에서 자바는 항상 앞서갔다
- 자바 8부터 `parallel()`만 한 번 호출하면 파이프라인을 병렬 실행할 수 있는 스트림을 지원하기 시작

<!-- 동시성 프로그래밍을 할 때는 `안전성(Safety)`과 `응답가능(liveness)` 상태를 유지해야 하는데 병렬 스트림 파이프라인 프로그래밍에서도 마찬가지 -->

하지만, 환경이 아무리 좋더라도 
  - 데이터 소스가 [`Stream.iterate()`](https://www.daleseo.com/java9-stream-iterate/)거나 
  - 중간 연산으로 `limit()`를 사용하면 <br>

  __파이프라인 병렬화로는 성능 개선을 기대할 수 없다__
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
  ➡️ 마구잡이로 병렬화하면 성능이 오히려 나빠질 수 있다

  > ## 병렬화 효율이 좋은 자료구조
  스트림의 소스가 다음과 같을때 병렬화 효과가 좋다
  - ArrayList, HashMap, HashSet, [ConcurrentHashMap](https://devlog-wjdrbs96.tistory.com/269)의 인스턴스 
  - 배열 , int, long 범위

 ➡️ 위 자료구조들은 모두 데이터를 원하는 크기로 정확하고 손쉽게 나눌 수 있어 다수의 스레드에 분배하기에 좋고,<br> 
 `참조 지역성(locality of reference)` 이 높아 성능이 좋다
  * 참조 지역성 : 이웃원소의 참조들이 메모리에 연속해서 저장되어 있다
  * `기본 타입 배열`은 (참조가 아닌) 데이터 자체가 메모리에 연속되어 저장되므로 참조지역성이 가장 뛰어난 자료구조이다

 참조 지역성이 낮으면 스레드는 주 메모리에서 캐시 메모리로 전송되기를 기다리며 대부분 시간을 멍하니 보내게 된다 <br>
 따라서 참조 지역성은 벌크 연산을 병렬화할 때 아주 중요한 요소로 작용한다
 



