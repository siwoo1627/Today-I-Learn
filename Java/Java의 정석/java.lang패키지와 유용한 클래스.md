## java.lang패키지와 유용한 클래스

Object 클래스는 모든 클래스의 조상이다.

### equals()

equals 메서드는 참조변수의 주소값으로 비교함

```java
// 오버라이딩
    public boolean equals(Object obj) {
        if(obj instanceof Person)
            return id ==((Person)obj).id;
        else
            return false;
    }
```

### String 클래스

덧셈 연산자 `+`를 사용해서 문자열을 결합하는 것은 매 연산 시 마다 새로운 문자열을 가진 String인스턴스가 생성되어 메모리공간을 차지하게 되므로 **문자열을 다루는 작업이 많이 필요한 경우에는 `StringBuffer`클래스를 사용하는 것이 좋다.**

```java
String a = "a";
String b = "b";
a = a + b; // ab값에 해당되는 a의 새로운 메모리공간이 생성됨
```

### 문자열 비교

```java
String str1 = "abc"; // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str2 = "abc";
String str3 = new String("abc"); // 새로운 String 인스턴스를 생성
String str4 = new String("abc");

str1 == str2;// true
str3 == str4;// false
str1.equals(str2); // true
str3.equals(str4); // true
```

### StringBuffer

 String 클래스에서는 equals메서드가 오버라이딩되어 있지만 StringBuffer클래스는 되어 있지 않아 등가연산자(==)와 동일한 결과가 나온다.
 반면에 toString()은 오버라이딩되어 있어서 toString()을 통해 String으로 변환 후 비교한다.

```java
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

Systerm.out.println(sb==sb2); // false
Systerm.out.println(sb.equals(sb2)); // false 

// StringBuffer의 내용을 String으로 변환한다.
String s  = sb.toString();    // String s = new String(sb);와 같다.
String s2 = sb2.toString();

System.out.println(s.equals(s2)); // true
```

### Math클래스

![Math메서드](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/ddfa7cb8-9628-4ac2-b339-81abc93c6747)

### wrapper클래스

기본형 변수를 객체로 변환할 때 사용하는 변수

> 매개변수로 객체를 요루, 기본형 값이 아닌 객체로 저장해야할 때, 객체간의 비교가 필요할 때 등

```java
Integer i = new Integer(100);
Long l = new Long(100);
```

- 문자열을 숫자로 변환
  
  ```java
  int i = Integer.parseInt("100"); // 문자열을 기본형으로 변환
  Integer i = Integer.valueOf("100") // 문자열을 래퍼 클래스 타입으로 반환
  ```


