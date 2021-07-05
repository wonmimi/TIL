# 아이템 13. clone 재정의는 주의해서 진행해라

> 객체를 복제 하기위해 clone 메서드의 구현방법과 다른 선택지에 대해 논의 해보자

## _cloneable_
복제가능한 클래스임을 명시하는 용도의 믹스인 인터페이스
```java
public interface Cloneable {
}
```

- _clone메서드_ 는 [Object](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html) 클래스에 정의 되어있는 클래스이다.

 ```java
 @HotSpotIntrinsicCandidate
  protected native Object clone() throws CloneNotSupportedException;
 ``` 
- protected인 clone 메서드는 구현만으로는 외부객체에서 clone메소드를 호출 할 수 없다.
    * protected : 같은 패키지나 상속관계의 클래스에서 접근 가능하고 그 외 외부에서는 접근 할 수 없다
- 상위클래스의 정의된 <b>clone 메서드를 변경하여 동작 방식을 결정</b> 한다 

- cloneable을 구현하지않은 클래스의 인스턴스에서 clone을 호출하면 CloneNotSupportedException을 던진다 (이례적인 인터페이스 사용 예시)


## _clone 메서드 구현_
### a) 가변 상태 참조하지 않는 클래스 
- 필드가 기본타입 이거나 불변객체를 참조한다면 super.clone 호출로 충분하다
```java
public class User implements Cloneable{
    private  String name;

    public User(String name){
        this.name = name;
    }
    
    ...getter...

    @Override
    public User clone(){
        try {
            return (User)super.clone(); // 📌 1️⃣
        } catch (CloneNotSupportedException e){ // check Exception (item 71)
            e.printStackTrace();
            throw new AssertionError();
        }
    }

    public static void main(String[] args) {
        User s1 = new User("wonmimi");
        User s2 = s1.clone();
        System.out.println(s1.getName()+", "+s2.getName());

        // 규약 (필수로 참인것은 아니다)
        System.out.println(s2 != s1);
        System.out.println(s2.getClass() == s1.getClass());
        System.out.println(s1.clone().getClass() == s1.getClass());
    }
}
```
- 1️⃣ : Object가 아닌 복사한 클래스타입으로 반환 (권장) <br>
  => 클라이언트가 형변환 하지않아도 되게끔 하위타입으로 반환 

<img width="40%" alt="clone" src="https://user-images.githubusercontent.com/66981136/124473827-bee1a500-ddda-11eb-81a1-bce36abfdf89.png">

  
### b) 가변 상태 참조 클래스
```java
public class User implements Cloneable{
    private  String name;
    private Color color; // + 가변 상태 필드 📌

    public User(String name){
        this.name = name;
    }

    public User(String name, String color){
        this.name = name;
        this.color = new Color(name, color)
    }

    ...

    public static void main(String[] args) {
        User s3 = new User("wonmimi2","purple");
        User s4 = s3.clone();

        System.out.println(s3.getName()+", "+s4.getName());
        s3.color.setColor("yellow"); // 값 변경
        System.out.println(s3.color.getColor()+", "+s4.color.getColor());

        System.out.println(s3.color);
        System.out.println(s4.color);
    }
}

```

<img width="40%" alt="clone_2" src="https://user-images.githubusercontent.com/66981136/124474440-7b3b6b00-dddb-11eb-83db-67f8f469fe18.png">

- clone은 사실상 객체를 생성하는 생성자와 같은 효과를 내므로, 원본 객체에 영향 끼치지 않는 동시에 복제된 객체의 <b>불변식을 보장해야 한다</b>
- 클래스가 가변상태인 필드를 가지면 원본과 복제된 클래스가 동시에 수정되어 <b>불변식을 해치게된다</b>


> clone을 재귀적으로 호출하여 가변상태의 필드도 복사한다
```java
    @Override
    public User clone(){
        try {
            User clone = (User) super.clone();
            clone.color = clone.color.clone(); // 📌
            return clone;
        } catch (CloneNotSupportedException e){
            e.printStackTrace();
            throw new AssertionError();
        }
    }
```

<img width="40%" alt="clone_3" src="https://user-images.githubusercontent.com/66981136/124475505-c3a75880-dddc-11eb-81d6-9006bb1bba53.png">

- ex) stack
<!-- - final 필드는 새로운 값을 할당할수 없기 때문에 제거 해야 할수도 있다 -->

#### _재귀 호출만으로 충분하지 않을때_
key-value 쌍을 담은 연결리스트의 엔트리를 참조하는 버킷들의 배열로 이루어진 해시테이블 타입 클래스의 clone
- 복제본의 버킷배열이 원본과 같은 연결리스트를 참조 하게 된다

```java
public class HashTable implements Cloneable{

    private Entry[] buckets = new Entry[100];
    private int size = 0;

    private static class Entry {
        final Object key;
        Object value;
        Entry next;
    }
    ...
    @Override
    protected HashTable clone() {
        try {
            HashTable result = (HashTable) super.clone();
            result.buckets = result.buckets.clone(); // 재귀호출 clone
            return result;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

<img width="50%" alt="clone_hashtable" src="https://user-images.githubusercontent.com/66981136/124460502-67d3d400-ddca-11eb-8dfa-2cf632036297.png">

> 각 버킷을 구성하는 연결리스트들을 모두 복사해야한다
```java

    // 엔트리가 가르키는 연결리스트를 재귀적으로 복사 📌
    Entry deepCopy() { 
        return new Entry(key, value, next == null ? null : next.deepCopy());
    }

     @Override
    protected HashTable clone() {
        try {
            HashTable result = (HashTable) super.clone();
//            result.buckets = result.buckets.clone(); // 잘못된 clone 📌

            result.buckets = new Entry[buckets.length];
            for (int i = 0; i < buckets.length; i++) { // 📌
                if (buckets[i] != null)
                    result.buckets[i] = buckets[i].deepCopy();
            }
            return result;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
```
- clone메서드는 새로운 버킷배열을 할당 후 각 버킷에 깊은복사 (deep copy)를 수행한다
- Entry 자신을 재귀 호출하여 연결리스트 전체를 복사한다

<img width="50%" alt="clone_hashtable" src="https://user-images.githubusercontent.com/66981136/124466199-59d58180-ddd1-11eb-84ee-24c5aa63ce3a.png">


### c) 복사생성자, 복사 팩터리 방식으로 객체 복사
> _복사생성자_  <br>
자신과 같은 클래스의 인스턴스를 인수로 받는 생성자 
```java
  public Foo(Foo foo) { ... };
```
> _복사팩터리_ <br>
복사생성자를  [정적패터리](../2장/1_생성자_대신_정적_팩터리_메서드를_고려하라_최문규.md) 형식으로 정의
```java
  public static Foo newInstance(Foo foo) { ... };
```

Cloneable 방식에서 하였던 
- _위험한 객체 생성 메커니즘_(생성자를 사용하지 않고 객체 생성)을 사용하지 않는다
- check exception(CloneNotSupportedException) 처리할 필요 없다
- 복제할 클래스로의 형변환을 할 필요가 없다.
- 클래스가 구현한 인터페이스 타입의 인스턴스를 매개 변수로 받을 수 있다.
  * = 원본의 구현 타입과 상관없이 복제본의 타입을 직접 선택할 수 있다
 ```java 
  HashSet<String> hashSet = new HashSet<>();
  TreeSet<String> treeSet = new TreeSet<>(hashSet);
 ``` 

> cloneable을 이미 구현한 클래스를 확장한다면 clone을 재정의하여 구현하여야 한다. 하지만 결과적으로 객체의 복제 기능은 복사 생성자와 복사 팩토리를 이용하는 것이 가장 좋다.


- - - 


- [참고1](https://k3068.tistory.com/65)
- [참고2](https://incheol-jung.gitbook.io/docs/study/effective-java/undefined-1/2020-03-20-effective-13item)
- [참고3](https://insight-bgh.tistory.com/394)
- [참고4](https://mongsil1025.github.io/book/effective-java/item13/)