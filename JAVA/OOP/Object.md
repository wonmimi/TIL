## < 객체 >
- 데이터의 단위 ( Ex :  회원, 생산, 주문, 배송 )
- 자바 모든 클래스의 최상위 클래스 : Object

- 객체 지향 프로그래밍 구현 
  * 객체 정의 
  * 각 객체 제공하는 기능(속성 : property) 들을 구현
  * 각 객체가 제공하는 기능들 간의 소통(파라미터, 객체 ... 전달)을 통하여 객체간의 협력을 구현
- - - 
- [클래스](#1.-클래스)
- [함수와 메서드](#2.-함수와-메서드)
- [인스턴스](#3.-인스턴스-(instance))
- [생성자](#4.생성자-(constructor))
- [접근 제어 지시자 & 정보은닉](#5.-접근-제어-지시자와-정보은닉)
- [static](#6.-static)


### 1. 클래스
- 객체의 속성은 클래스의 멤버 변수(member variable)로, 역할을 메서드(method)로 선언하여 정의
  * 빈값으로 선언하였을때 멤버변수는 자동 초기화 (string : null, int : 0 <=> 지역변수 )
- 클래스명은 대문자로 시작 (CamelCase)
- 변수, 메서드명은 소문자로 시작
- 한 java 파일에  여러 클래스가 있을 수 있지만 public 클래스는 오직 하나
  * public 클래스 이름 =  .java 파일 이름
- - - 
### 2. 함수와 메서드
#### 2-1. 함수 (function)
- 기능을 실행하는 코드
- 호출하여 사용하고 호출된 함수는 기능이 끝나면 제어가 호출시점으로 반환됨
- 함수로 구현된 기능은 여러 곳에서 호출되어 사용될 수 있음
- 어딘가에 속하지않는 단독 모듈
#### 2-2. 함수 호출 스택(stack) 메모리
- 함수가 호출될 때 지역 변수들이 사용하는 메모리
- 함수 실행이 끝나면 자동으로 반환 되는 메모리


<img src = "../../img/java-function-stack.png" alt = "stack" width="50%" height="70%">

#### 2-3. 메서드 (method)
- 객체의 기능을 구현하기 위해 클래스 내부에 구현되는 함수 = 멤버 함수 (member function)
- 메서드의 이름은 그 객체를 사용(호출)하는 객체(=클라이언트)에 맞게 네이밍 (ex  getStudentName() )
- - - 
### 3. 인스턴스 (instance)
#### 3-1. 인스턴스 생성
- 인스턴스 : 클래스를 기반으로 new 키워드를 사용하여 메모리에 생성
  * 생성된 인스턴스는 각각 다른 멤버변수(참조변수) 값(참조값)을 가진다
    * 참조변수 : 메모리에 생성된 인스턴스를 가리키는 변수
    * 참조값 : 생성된 인스턴스의 메모리 주소 값
    ```java
       Student studentJung = new Student(); // 참조변수
       System.out.println(studentJung); //참조값 출력 
       //=> [패키지명].Student@1218025c  jvm이 부여한 가상주소값 
    ```

#### 3-2. 힙(heap) 메모리 (= 동적메모리)
- 생성된 인스턴스는 힙 메모리에 할당됨
- 자바에서 Gabage Collector 가 주기적으로 사용하지 않는 메모리를 수거
  * C, C++에서는 해제시켜야함 (free() , delete)
- 하나의 클래스에서 여러 인스턴스가 생성되면 각각 다른 메모리주소를 가짐

<img src = "../../img/java_stack_heap.png" alt = "stack" width="50%" height="60%">

- - - 
### 4.생성자 (constructor)
  - 생성자는 객체를 생성하기 위해 new 와 함께 호출
  - 객체가 생성될 때 변수, 상수를 초기화 하거나 초기화 기능을 하는 메서드를 호출
  - 생성자는 반환 값이 없고 클래스의 이름과 동일
  - 생성자는 보통 외부접근이 가능(public) 하지만 private 으로 선언되는 경우도 있음
  - 오버로딩(같은 클래스명, 파라미터 다름) 하여 생성자 여러개 정의 가능,  필요에 따라 호출하여 사용
  - 보통 0으로 시작하는 숫자를 표현할땐 String 타입을 사용
#### 4-1. 기본생성자 (default constructor)
- 클래스에는 적어도 하나 이상의 생성자가 존재
- 생성자를 구현하지 않아도 new 키워드와 함께 생성자를 호출
  * 컴파일러가 생성자 코드를 자동생성 
  ```java
  public className () { ... } 
  ```
- 매개 변수, 구현부가 없다
- 생성자 직접 구현 가능
#### 4-2. 참조자료형 변수
- 클래스형 변수 선언 하여 해당변수를 생성해여야 한다
  * (String 클래스는 예외적으로 생성하지않고 사용 가능)
- 기본 자료형은 사용하는 메모리의 크기가 정해져 있고, 참조 자료형은 클래스에 따라 다름
- - - 

### 5. 접근 제어 지시자와 정보은닉
#### ___5-1. 접근 제어 지시자(access modifier)___
- 클래스 외부에서 클래스의 멤버 변수, 메서드, 생성자를 사용 여부를 지정하는 키워드
- private : 같은 클래스 내부에서만 접근 가능 ( 외부 클래스, 상속 관계의 클래스에서도 접근 불가)
  * get() , set() : private 으로 선언된 멤버 변수에 대해 접근, 수정할 수 있는 메서드.  public으로 제공
  * get() 메서드만 제공 되는 건 read-only 필드
get() 메서드만 제공 되는 경우 read-only 필드
- 아무것도 없음 (default) : 같은 패키지 내부에서만 접근 가능 ( 상속 관계라도 패키지가 다르면 접근 불가 - import 추가 )
- protected : 같은 패키지나 상속관계의 클래스에서 접근 가능하고 그 외 외부에서는 접근 할 수 없음
- public : 클래스의 외부 어디서나 접근 할 수 있음

#### ___5-2. 정보 은닉___
외부에서 접근 가능한 최소한의 정보를 오픈하여 객체의 오류를 방지

ex) private 필드인 isValid로 정보은닉
```java
public void setMonth(int month) {
		
		if ( month < 1 || month > 12) {
			isValid = false;
		}
		else {
			this.month = month;
		}
	}

```
#### ___5-3 캡슐화 (encapsulation)___
정보은닉을 활용하여 필요한 정보와 기능만 외부에 오픈
- 대부분의 멤버 변수와 메서드를 감추고 외부에 통합된 인터페이스만 제공하여 client코드에서 일관된 기능을 구현
  * hide된 속성들은 client코드 쪽에선 알 필요가 없다.
- client코드 에서 메서드나 멤버변수에 접근함으로써 발생하는 오류 최소화 

- - - 
### 6. static 
여러 인스턴스에서 공통으로 사용하여 메모리 공유
#### 6-1 static (정적) 변수

- 처음 프로그램이 메모리에 로딩될 때 메모리를 할당하여 프로그램 종료시 해제

<img src = "../../img/static변수_memory.png" alt = "static" width="50%" height="60%">

- 인스턴스 생성과 상관 없이 사용 가능하므로 클래스 이름으로 직접 참조📌
- = 클래스 변수, 정적변수라고도 함(<-> 인스턴스 변수)

메모리 영역

<img src = "../../img/static변수_메모리.png" alt = "static" width="60%" height="60%">

#### 6-2. static 메서드 
- 인스턴스 생성과 무관하게 클래스 이름으로 호출( = 클래스 메서드 , 정적 메서드) 📌
- static 메서드 내부에서는 인스턴스 변수를 사용할 수 없음
  *  인스턴스 생성 전에 호출 될 수 있기 때문
- static 변수는 사용가능 
- this 키워드도 사용할수 없다

```java
public class Employee {

	private static int serialNum = 1000; 
	
	private int employeeId;
	private String employeeName;
	private String department;

    public static int getSerialNum() {
      return serialNum;
    }

    public static void setSerialNum(int serialNum) {
      Employee.serialNum = serialNum; 📌
    }

    ...
}

public class EmployeeTest {

	public static void main(String[] args) {
		Employee employeeLee = new Employee();
		employeeLee.setEmployeeName("이순신");
    System.out.println(Employee.getSerialNum());
		
		Employee employeeKim = new Employee();
		employeeKim.setEmployeeName("김유신");
		
	}
}

```

#### 6-3. 변수 유효범위(scope)와 메모리
지역변수, 멤버 변수, 클래스 변수는 유효범위와 life cycle, 사용하는 메모가 다름

<img src = "../../img/static_변수별.png" alt = "static" width="60%" height="60%">

- static 변수는 프로그램 실행동안 메모리영역을 차지하므로 너무 큰 메모리를 할당하지 않도록 유의
- 클래스 내부의 여러 메서드에서 사용하는 변수는 멤버 변수로 선언
- 멤버 변수가 너무 많으면 인스턴스 생성 시 불필요한 메모리가 할당

#### 6-4. 싱글톤 패턴 (Singleton pattern)
프로그램에서 인스턴스(객체)가 단 한 개만 생성되어야 하는 경우 사용하는 디자인 패턴
- 서로 자원을 공유할때 주로 사용 ex) 스프링 빈 , (현실)프린터 기계
- 객체 구현  
  - static 변수, 메서드를 활용하여 구현 할수있음
  - 생성자는 private으로 선언
  - 클래스 내부에 유일한 private 인스턴스 생성
  - 외부에서 유일한 인스턴스를 참조할 수 있는 public 메서드 제공
- - - 

### final 키워드
변수나 메서드 또는 클래스가 ‘변경 불가능’하도록 만든다.
- 원시(Primitive) 변수일 때  해당 변수의 값은 변경이 불가능하다.
- 참조(Reference) 변수일 때 힙(heap) 내의 다른 객체를 가리키도록 변경할 수 없다.
- 메서드에 적용할 때 해당 메서드를 오버라이드할 수 없다.
- 클래스에 적용 시 해당 클래스의 하위 클래스를 정의할 수 없다.(= 상속받을수 없다)







