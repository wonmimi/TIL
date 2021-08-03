##  배열 (Array)
> 자료를 순차적으로 한꺼번에 관리
- 동일한 자료형의 순차적 자료 구조
- 물리적 위치와 논리적 위치가 동일
- 인덱스 연산자`[]`를 이용하여 빠른 참조가 가능 `ex)  arr[1] `
  * 배열의 순서 시작 0 부터
  * []를 활용하여 배열 요소가 저장된 메모리의 위치를 찾음
- 요소 삽입,삭제에 할때 O(n)
  * 보완한게 `ArrayList` (java.util 패키지에서 구현한 객체배열)
  * 자바에서는 객체 배열을 구현한 `ArrayList`를 많이 활용

> 배열 선언 & 초기화

__선언__
- 배열은 선언과 동시에 자료형에 따라 초기화 됨 
  *  정수는 0, 실수는 0.0, 객체는 null
```java
int[] arr1 = new int[7];
int arr2[] = new int[7];
```
__초기화__
```java
int[] arr = new int[] {1, 2, 3};  //개수 생략 가능
int[] arr = {1, 2, 3};  // new int[]  생략 가능 

int[] ids;  // 선언후 배열을 생성하는 경우
ids = new int[] {10, 20, 30}; // new int[] 생략 X
```

>배열의 길이(length)와 요소(elements)의 개수는 항상 동일하지 않다

- 배열을 선언하면 개수만큼 메모리가 할당되지만 실제 요소(데이터)가 없는 경우도 있다
- 배열의 `length` 속성은 배열의 개수를 반환해주기 때문에 요소의 개수와는 다름

> __enhanced for__ 문 활용 

배열의 n개 요소를 `0 ~ n-1`까지 순차적으로 순회
```java
for(변수타입 변수 : 배열) {
  ...
}
```
예시 코드
```java
for(int value : arr){
    total += value;
}

```

### 객체 배열 
객체 배열은 요소가 되는 객체의 주소가 들어갈(4바이트, 8바이트) 메모리만 할당되고 각 요소 객체는 생성하여 저장해야 함 (초기 null)
  * 기본 자료형 배열은 선언과 동시에 배열의 크기만큼의 메모리 할당

```java
  Book[] library = new Book[5];
```
<img width="70%" alt="ObjectArray" src="https://user-images.githubusercontent.com/66981136/126354929-d71c7764-370a-426b-9f98-c9b557ada102.png">

객체를 생성하여 각 배열의 요소로 저장하기
```java
  Book[] library = new Book[5];
  Book[] copylibrary = new Book[5];

  library[0] = new Book("책1","작가1");
  library[1] = new Book("책2","작가2");
  ...
```

> 객체 배열 복사

__얕은 복사__

 `System.arrayCopy`(src, srcPos, dest, destPos, length) 자바 제공하는 배열 복사 메서드
 ```java
  System.arraycopy(library,0,copyLibrary,0,5);
 ```

- 객체 주소만 복사되어 한쪽 배열의 요소를 수정하면 같이 수정된다
  * 두 배열이 같은 객체를 가리킴

__깊은 복사__

각각 생성한 객체에 기존 객체의 값을 복사하여 배열이 서로 다른 객체를 가리키도록 함
```java
  copyLibrary[0] = new Book();
  copyLibrary[1] = new Book();
  ...

  for(int i = 0 ; i < library.length; i++){
    copyLibrary[i].setTitle(library[i].getTitle());
    copyLibrary[i].setAuthor(library[i].getAuthor());
  }
```
💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter02/ch21)

> 다차원 배열
- 이차원 이상으로 구현 된 배열
- 평면 (이차원 배열) 이나 공간(삼차원 배열)을 활용한 프로그램 구현
```java
  int[][] arr = new int[2][3];
```
<img width="70%" alt="2차원배열" src="https://user-images.githubusercontent.com/66981136/126509030-930d2acd-5fea-44c7-b900-ae430955da55.png">


> ArrayList 

객체 배열을 구현한 클래스
- 기존의 배열 방식은 배열의 길이를 정하고 요소의 개수가 배열의 길이보다 커지면 배열을 재할당하고 복사해야 한다
- 배열의 요소를 추가 또는 삭제하면 다른 요소들의 이동 필요
- `ArrayList는 객체 배열을 좀더 효율적으로 관리`하기 위해 자바에서 제공하는 클래스
  * 많은 메서드들이 최적의 알고리즘으로 구현되어 있어 각 메서드의 사용 방법 익힐수록 유용

> [__주요 메서드__](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
  
<img width="75%" alt="arrayList" src="https://user-images.githubusercontent.com/66981136/126519211-fca04e3b-81e8-4d01-8c8c-f3b9fe33ec60.png">

```java
  ArrayList<Book> library = new ArrayList<>();
  library.add(new Book("제목", "작가")); 📌

  Book book1 = library.get(1); 📌

```
💻 [예제 코드](https://github.com/wonmimi/java-programming-skills/tree/main/src/GrammarPractice/Chapter02/ch24)