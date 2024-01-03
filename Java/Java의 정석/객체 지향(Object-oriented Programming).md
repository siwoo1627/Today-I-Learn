# 객체 지향(Object-oriented Programming)

객체: 실제하는 것

- 속성 -> 멤버변수
- 기능 -> 메서드

클래스: 설계도

* * *

### 클래스(class)

- 클래스로부터 객체를 만드는 과정을 **클래스의 인스턴스화** 라고 한다.
  
  - 만들어진 객체를 **클래스의 인스턴스**라고 한다.

```java
클래스명 변수명;            // 클래스의 객체를 참조하기 위한 참조변수를 선언
변수명 = new 클래스명()    //클래스 객체를 생성 후, 객체의 주소를 참조변수에 저장
```

- 객체배열

```java
Tv[] tvArr new Tv[3];    // 객체를 다루기 위한 참조변수들이 만들어질 뿐 객체가 생성되지 않음
tvArr[0] = new Tv();
tvArr[1] = new Tv();
tvArr[2] = new Tv();
Tv[] tvArr = {new Tv(),new Tv(),new Tv()};
```

- 클래스 변수와 인스턴스 변수
  - 인스턴수 변수
    - 클래스 내에서만 사용하는 변수
    - 생성시기: 인스턴스가 생성될때
  - 클래스 변수
    - 모든 인스턴스가 하나의 저장공간을 공유하므로 함께 변함
    - 생성시기: 클래스가 메모리에 올라갈때(객체 생성없이 사용가능)

```java
class Variavles{
    int iv;            // 인스턴스 변수
    static int cv;    // 클래스 변수(static 변수, 공유변수)
}
```

- 기본형 매개변수 / 참조형 매개변수

```java
void change(int x){    // 기본형 매개변수 x=10;
    x=1000;
    System.out.println(x);
}
void change(Data d){ // 참조형 매개변수 x=1000;으로 변경됨
    d.x = 1000;
    System.out.println(d.x);
}
```

- 클래스 메서드(static 메서드) / 인스턴스 메서드

```java
satic int add(int a, int b) {return a+b}
// static변수와 동일하게 객체생성하지 않아도 사용가능
```

- static을 언제 붙여야 할까
  - 클래스를 설계할 때, 멤버변수 중 모든 인스턴스에 공통으로 사용하는 것에 static을 붙인다.
  - 클래스 변수는 인스턴스를 생성하지 않아도 사용할 수 있다.
    - static이 붙은 변수는 클래스가 메모리에 올라갈 때 이미 자동적으로 생성되기 때문
  - 클래스 메서드는 인스턴스 변수를 사용할 수 없다
    - 클래스 메서드를 호출했을 때 인스턴스가 존재하지 않을 수도 있어서
  - 메서드 내에서 인스턴스 변수를 사용하지 않는다면, static을 붙이는 것을 고려한다
    - 클래스가 메모리에 올라갈 때 생성되므로 메서드를 찾는 과정이 사라짐

* * *

### 오버로딩(overloading)

한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것

- 메서드의 이름이 같아야함
- 매개변수의 개수 또는 타입이 달라야 함
- 반환 타입은 관계없음

```java
long add(int a, int b){return a+b;}    // 같은 이름이지만 매개변수 타입이 다름
long add(long a, long b){return a+b;}
```

* * *

### 생성자(constructor)

인스턴스가 생성될 때 호출되는 `인스턴스 초기화 메서드`

- 생성자의 이름은 클래스의 이름과 같아야 한다.

- 생성자는 리턴 값이 없다.

> 연산자 new가 인스턴스를 생성하는 것이지 생성자가 인스턴스를 생성하는 것이 아니다.
> 생성자는 단순히 인스턴스변수들의 초기화에 사용되는 조금 특별한 메서드이다.

기본 생성자

- 생성자가 하나도 없을 경우 컴파일러가 자동으로 추가해준다

```java
클래스 이름(){}    // 기본 생성자
class Point{
    Point(){}
}
```

매개변수가 있는 생성자

- 인스턴스를 생성하는 동시에 원하는 값으로 초기화됨

```java
class Car{
    String color;
    int door;

    Car(String c, int d){
        color = c;
        door = d;
    }
}

Car c = new Car("red",4);
```

this / this();

- this: 인스턴스 자신을 가리키는 참조변수, 인스턴스의 주소가 저장되어 있다.
- this(): 생성자, 같은 클래스의 다른 생성자를 호출할 때 사용한다.

* * *

### 클래스의 재사용

- 상속
  
  > '은 ~이다(is-a)'

```java
class Parent{ }
class Child extends Parent{}
```

포함(composite) 관계: 한 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언

> '~은 ~을 가지고 있다(has-a)'

```java
class Circle{
    int x;
    int y;
    int r;
}
class Point{
    int x;
    int y;
}
// 포함
class Circle{
    Point p = new Point();
    int r;
}
// 상속
class Circle extends Pont{
    int r;
}
```

오버라이딩(overriding)

- 조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것
  
  - 오버라이딩은 메서드의 내용만 새로 작성하는 것이므로 메서드의 선언부는 조상의 것과 완전 일치해야 한다.
  
  - 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.
  
  - 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
    
    > protected -> private는 안됨

**오버로딩 vs 오버라이딩**

- 오버로딩:        기존에 없는 새로운 메서드를 정의하는 것
- 오버라이딩:    상속받은 메서드의 내용을 변경하는것

* * *

### 패키지

클래스의 묶음이다, 클래스가 물리적으로 하나의 클래스파일(.class)인 것과 같이 패키지는 물리적으로 하나의 디렉토리이다.

```java
package 패키지명;
```

> String클래스는 java.util.String이다.

### import

클래스의 패키지 명을 미리 명시해주어 클래스이름에서 패키지명을 생략할 수 있다.

```java
java.util.Date d = new java.util.Date();

import java.util;
import java.*;
Date d = new Date();
```

### static import

static멤버를 호출 할 때 클래스의 이름을 생략할 수 있다.

```java
import static java.lang.System.out;
import static java.lang.Math.random;

// 아래 두 코드는 동일
System.out.println(Math.random());
out.println(random());
```

* * *

### 캡슐화와 접근제어자

접근제어자를 사용하는 이유

- 외부로붙터 데이터를 보호하기 위해서
- 외부에는 불필요한, 내부적으로만 사용되는, 부분을 감추기 위해서

```java
public class time{
    private int hour;
    // 상속에서 사용할 경우 자식클래스 접근을 위해 protected로 변경
    public void setHour(int hour){
        this.hour = hour;
    }
}
```

### 다형성

서로 상속관계에 있을 경우, 조상 클래스 타입의 참조변수로 자손 클래스의 인슽턴스를 참조하도록 하는 것이 가능

```java
Tv t = new SmartTv(); // 조상타입 참조변수 = new 자손 타입 인스턴스 참조();
// Tv 클래스에 정의되지 않은 데이터와 메서드는 참조변수 t로 사용불가
```

### 참조변수의 형변환

조상타입 <-> 자식타입

> 자식타입 <-> 자식타입 은 안됨

```java
class Ex7_7 {
    public static void main(String args[]) {
        Car car = null;
        FireEngine fe = new FireEngine();
        FireEngine fe2 = null;

        fe.water();
        car = fe;    // car = (Car)fe;에서 형변환이 생략됨
//        car.water();
        fe2 = (FireEngine)car; // 자손타입 ← 조상타입. 형변환 생략 불가
        fe2.water();
    }
}

class Car {
    String color;
    int door;

    void drive() {     // 운전하는 기능
        System.out.println("drive, Brrrr~");
    }

    void stop() {      // 멈추는 기능    
        System.out.println("stop!!!");    
    }
}

class FireEngine extends Car {    // 소방차
    void water() {    // 물을 뿌리는 기능
        System.out.println("water!!!");
    }
}
```

* * * 

### 추상 클래스, 추상 메서드

추상 클래스: 인스턴스 생성불가, 미완성 설계도

```java
absctract class 클래스이름{ }
```

추상메서드

- 메서드 선언을 통해 상속받는 자손클래스에 추상 메서드를 반드시 오버라이딩해야한다.

```java
// 구현부가 없으므로 {} 대신 ;를 붙여준다.
abstract 리턴타입 메서드이름();
```

* * *

### 인터페이스

인터페이스: 일종의 추상클래스, 기본설계도

- 모든 멤버변수는 public static final이어야하고 생략가능

- 모든 메서드는 public abstract이고 생략가능
  
  > 단, static메서드와 디폴트 메서드는 예외

```java
interface 인터페이스이름{
    public static final 타입 상수이름 = 값;
    public abstract 메서드이름(매개변수목록);
}
```

인터페이스부터만 상속가능하며 다중상속가능

```java
interface Fightable extends Movalbe, Attackable{}
```

인터페이스 구현

```java
class 클래스 이름 (extends 조상클래스) implements 인터페이스이름{
    // 인터페이스에 정의된 추상메서드를 모두 구현해야한다.
}
```

* * *

### 내부클래스(inner class)

클래스 내에 선언된 클래스

- 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
- 코드의 복잡성을 줄일 수 있다(캡슐화).

```java
class A{
    class B{
    }
}
```

| 내부클래스    | 특징                                           |
| -------- | -------------------------------------------- |
| 인스턴스 클래스 | 외부클래스의 멤버변수 위치에 선언하며, 인스턴스 멤버처럼 다루어짐         |
| 스태틱 클래스  | 외부클래스의 멤버변수 위치에 선언하며, 외부 static 멤버처럼 다루어짐    |
| 지역 클래스   | 외부 클래스의 메서드나 초기화 블럭 안에 선언하며, 선언된 영역 내부에서만 사용 |
| 익명 클래스   | 클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스(일회용)        |

```java
class Outer2 {
    class InstanceInner {    // 인스턴스 클래스
        int iv = 100;
    }

    static class StaticInner {    // 스태틱 클래스
        int iv = 200;
        static int cv = 300;
    }

    void myMethod() {
        class LocalInner {    // 지역 클래스
            int iv = 400;
        }
    }
}

class Ex7_15 {
    public static void main(String[] args) {
        // 인스턴스클래스의 인스턴스를 생성하려면
        // 외부 클래스의 인스턴스를 먼저 생성해야 한다.
        Outer2 oc = new Outer2();
        Outer2.InstanceInner ii = oc.new InstanceInner();

        System.out.println("ii.iv : "+ ii.iv);
        System.out.println("Outer2.StaticInner.cv : "+Outer2.StaticInner.cv);

       // 스태틱 내부 클래스의 인스턴스는 외부 클래스를 먼저 생성하지 않아도 된다.
        Outer2.StaticInner si = new Outer2.StaticInner();
        System.out.println("si.iv : "+ si.iv);
    }
}
```

```java
import java.awt.*;
import java.awt.event.*;

class Ex7_19 {
    public static void main(String[] args) {
        Button b = new Button("Start");
        b.addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    System.out.println("ActionEvent occurred!!!");
                }
            } // 익명 클래스의 끝
        );
    } // main의 끝
} 
```


