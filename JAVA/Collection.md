# 자료구조 (Data Structure)
- 프로그램에서 사용할 데이터들을 메모리에서 관리하는 여러 구현방법
- 효율적인 자료구조 => 좋은 성능을 가진 알고리즘의 기반
  * => 프로그램의 수행속도와도 밀접한 관련
---
## 컬렉션 프레임워크 (Collection framework)
- 자료구조를 구현해 놓은 JDK 라이브러리
- `java.util` 패키지에 구현
- 최적화 된 알고리즘을 사용할 수 있도록 한다
- 여러 구현 클래스와 인터페이스를 활용 하여야 한다
- <img width="85%" alt="" src="https://user-images.githubusercontent.com/66981136/126039173-05c5c469-7c20-4f02-a796-6d2f83e72df0.png">
- [+ 추가](https://velog.io/@jyo925/Collections-%ED%81%B4%EB%9E%98%EC%8A%A4)
- Collection 프레임워크 vs Collections 클래스 [차이](https://live-everyday.tistory.com/85)


## Collection 인터페이스
 하나의 요소(element)를 관리
  * [docs](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/Collection.html)
> ### List 인터페이스
- 객체를 __`순서`에 따라__ 저장하고 관리하는 메서드가 선언된 인터페이스
- `중복을 허용`함

- 자료구조 `리스트` (배열, 연결리스트)의 구현을 위한 인터페이스
- `ArrayList` ,LinkedList, Stack, Queue, Vector 등...


  ###  ArrayList 클래스
  기존의 [배열](./Array.md) 방식은 
  - 배열의 길이를 정하고 요소의 개수가 배열의 길이보다 커지면 배열을 재할당하고 복사해야 한다
  - 배열의 요소를 추가 또는 삭제하면 다른 요소들의 이동 필요

  `ArrayList는 객체 배열을 좀더 효율적으로 관리`하기 위해 자바에서 제공하는 클래스
    * 많은 메서드들이 최적의 알고리즘으로 구현

  __[ 많이 쓰는 메소드 ]__
  - <img width="75%" alt="arrayList" src="https://user-images.githubusercontent.com/66981136/126519211-fca04e3b-81e8-4d01-8c8c-f3b9fe33ec60.png">

  ```java
  ArrayList<Member> arrayList = new ArrayList<>();
  Member member1 = new Member(1001,"Jung");
  arrayList.add(member1);

  for (int i =0; i<arrayList.size(); i++){
    Member member = arrayList.get(i);

    int tempId = member.getMemberId();
    if(tempId == memberId){
        arrayList.remove(i);
        return true;
    }
  }
  ```
  - 그외 [docs](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/ArrayList.html) 참고
  - 💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter05/ch10_List)


---

> ### Set 인터페이스
- __`순서와 관계없이`__ `중복을 허용하지 않고` 유일한 값을 관리하는 메서드가 선언된 인터페이스. 집합
  * ex 아이디, 주민번호, 사번등을 관리하는데 유용
- 저장된 순서와 출력되는 순서는 다를 수 있음
- `HashSet`, [TreeSet](#treeset-클래스) 등...
  * hash  : 검색을 위한 알고리즘
  * tree : 이진 탐색 트리 (binary searchTree)사용. 정렬 알고리즘 (log2n)

  ### 🏷 HashSet 클래스
  Set 인터페이스를 구현
  - 인스턴스의 동일성 구현을 위해 필요에 따라 `equals()`와 [`hashCode()`](https://reakwon.tistory.com/83)메소드를 재정의함
    * jdk내부에 hashCode() 내장 되어있다 (String , Integer ... )

  - 💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter05/ch11_Set)
  
---
##  Map 인터페이스
- 쌍(pair)으로 이루어진 객체를 관리하는 메서드들이 선언된 인터페이스
  *  [docs](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/Map.html)
- `key-value의 쌍`인 객체로 이루어짐
- key는 `중복을 허용하지 않음`. 유일
- HashTable, `HashMap`, Properties, [`TreeMap`](#treemap-클래스) 등이 Map 인터페이스를 구현 함
- map의 내부 인터페이스인 [Map.Entry](https://codedragon.tistory.com/6046) 인터페이스

  ### 🏷 HashMap 클래스
  Map 인터페이스 구현
  - 검색을 위한 자료구조
  - key를 이용하여 값을 저장( `put(키, 값)`)하고 key를 이용하여 값을 꺼내오는 방식( `get(키)` ) 
    *  hash 알고리즘으로 구현
  - key가 되는 객체는 중복될 수 없고 __객체의 유일성을 비교__ 를 위해 필요시 `equals()`와 `hashCode() `메서드 재정의  ( = hashSet )

  - 💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter05/ch14_HashMap)
  - [ 맵 출력 ](https://stove99.tistory.com/96) /  [ 맵 정렬 - 값 ](https://junghn.tistory.com/entry/JAVA-Map%EC%97%90%EC%84%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC-%EA%B0%92Value%EA%B8%B0%EC%A4%80%EC%9C%BC%EB%A1%9C-%EC%A0%95%EB%A0%AC%EB%B0%A9%EB%B2%95-%EC%98%A4%EB%A6%84%EC%B0%A8%EC%88%9C-%EB%82%B4%EB%A6%BC%EC%B0%A8%EC%88%9C)

---
> ### length, length(), size() 차이
- length
  -  arrays(int[], double[], String[])
  - length는 `배열`의 길이를 알고자 할때 사용된다.
- length()
  - String related Object(String, StringBuilder etc)
  - length()는 `문자열`의 길이를 알고자 할때 사용된다.
- size()
  - Collection Object(ArrayList, Set etc)
  - size()는 `컬렉션프레임워크 타입`의 길이를 알고자 할때 사용된다.

```java
int[] arr = new int[7];
System.out.println( arr.length );  // 7

String str = "java";
System.out.println( str.length() );  // 4

ArrayList<Object> obj = new ArrayList<Object>();
System.out.println( obj.size() );  // 0
```
---
## Iterator
Collection 요소를 순회 = 컬렉션 프레임워크에 저장된 요소들을 하나씩 차례로 참조
- java.util 패키지
- 순서가 있는 List인터페이스의 경우는 Iterator를 get(i)로 접근 가능
- Set 인터페이스의 경우 get(i) 메서드가 제공되지 않으므로 Iterator 활용
- Map은 KeySet() , EntrySet(), values() ... 으로 활용

__주요 메소드__
- boolean `hasNext()` : 이후에 요소가 더 있는지 체크 있으면 true
- E `next()` : 다음 요소 반환
```java
  Iterator<Member> ir = arrayList.iterator();
  while (ir.hasNext()){
      Member member = ir.next();
      ...
  }
```
---

## 정렬을 위한 Comparable과 Comparator 인터페이스
- Comparable 인터페이스
  * 정렬 수행시 기본적으로 적용되는 정렬 기준이 되는 메서드를 정의해 놓는 인터페이스
  * 정렬 기준 변경시 `compareTo` 메소드 재정의 
  * `java.lang`패키지

- Comparator 클래스
  * 정렬 가능한 클래스(=Comparable이 구현된 클래스)들의 기본 정렬 기준과는 다른 방식으로 정렬하고 싶을 때 사용하는 클래스
  * `compare` 메소드 재정의
  * `java.util` 패키지
  * ```java
      class MyCompare implements Comparator<String>{...}
      // Comparator를 구현(compare()을 실행하는)하는 대상의 생성자 필수
      Set<String> set = new TreeSet<String>(new MyCompare());
    ```

[참고1](https://st-lab.tistory.com/243) /
[참고2](https://m.blog.naver.com/occidere/220918234464) /
[참고3](http://tcpschool.com/java/java_collectionFramework_comparable)

> ### TreeSet 클래스
객체의 정렬에 사용하는 클래스(sorted 계열))
- Set 인터페이스를 구현하여 중복을 허용하지 않고, 오름차순이나 내림차순으로 객체를 정렬
- 내부적으로 `이진검색트리(binary search tree)`로 구현됨
- 이진검색트리에 저장하기 위해 각 객체를 비교해야 함
- 바교 대상이 되는 객체에 `Comparable`이나 `Comparator` 인터페이스를 구현 해야 TreeSet에 추가
  * String, Integer등 JDK의 많은 클래스들이 이미 Comparable을 구현 (기본 오름차순)

- 💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter05/ch13_TreeSet)


> ### TreeMap 클래스
  &nbsp; Map 인터페이스를 구현 ( [TreeSet](#treeset-클래스) + [HashMap](#map-인터페이스) )
  - key가 되는 클래스에 Comparable이나 Comparator인터페이스를 구현함으로써 ( => treeSet)  <br>key-value 쌍의 자료를 key값 기준으로 정렬하여 관리 할 수 있음 ( => hashMap )
  
