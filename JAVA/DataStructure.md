## 자료구조 (Data Structure)
- 프로그램에서 사용할 데이터들을 메모리에서 관리하는 여러 구현방법
- 효율적인 자료구조 => 좋은 성능을 가진 알고리즘의 기반
  * => 프로그램의 수행속도와도 밀접한 관련

## 컬렉션 프레임워크 (collection framework)
- 자료구조를 구현해 놓은 JDK 라이브러리
- java.util 패키지에 구현
- 최적화 된 알고리즘을 사용할 수 있도록 한다
- 여러 구현 클래스와 인터페이스의 활용 하여야 한다

<img width="60%" alt="스크린샷 2021-07-17 오후 10 36 58" src="https://user-images.githubusercontent.com/66981136/126039173-05c5c469-7c20-4f02-a796-6d2f83e72df0.png">

### Collection 인터페이스
하나의 요소(element)를 관리
> List 인터페이스
- 객체를 `순서`에 따라 저장하고 관리하는 메서드가 선언된 인터페이스
- `중복을 허용`함

- 자료구조 `리스트` (배열, 연결리스트)의 구현을 위한 인터페이스
- `ArrayList`, LinkedList, Stack, Queue, Vector 등...

> Set 인터페이스
- `순서와 관계없이` `중복을 허용하지 않고` 유일한 값을 관리하는 메서드가 선언된 인터페이스. 집합
  * ex 아이디, 주민번호, 사번등을 관리하는데 유용
- 저장된 순서와 출력되는 순서는 다를 수 있음
- `HashSet`, TreeSet등...
  * hash  : 검색을 위한 알고리즘
  * tree : 이진 탐색 트리 (binary searchTree)사용. 정렬 알고리즘 (log2n)

> Map 인터페이스
- 쌍(pair)으로 이루어진 객체를 관리하는 메서드들이 선언된 인터페이스
- 객체는 `key-value의 쌍`으로 이루어짐
- key는 `중복을 허용하지 않음`
- HashTable, `HashMap`, Properties, TreeMap 등이 Map 인터페이스를 구현 함

[ 맵 출력 ](https://stove99.tistory.com/96)

