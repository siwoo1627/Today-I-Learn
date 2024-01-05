### 예외처리 (Exception handleing)

- 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1){
    // Exception1이 발생할 경우 이를 처리하기 위한 문장을 넣는다.
} finally{
    // 예외 발생 여부 관계없이 실행될 문장
}
```

```java
class Ex8_2 {
    public static void main(String args[]) {
            System.out.println(1);
            try {
                System.out.println(0/0);
                System.out.println(2);     // 실행되지 않는다.
            } catch (ArithmeticException ae)    {
                System.out.println(3);
            }    // try-catch의 끝
            System.out.println(4);
    }    // main메서드의 끝
}
```

- 모든 예외 클래스는 Exception 클래스의 자손이므로, catch 블럭 괄호 안에 Exception클래스 타입의 참조변수를 선언해 놓ㄹ으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.

`printStackTrace()`: 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외메시지를 화면에 출력한다.
`getMessage()`: 발생한 예외클래스의 인슽턴스에 저장된 메세지를 얻을 수 있다.

```java
class Ex8_5 {
    public static void main(String args[]) {
        System.out.println(1);            
        System.out.println(2);

        try {
            System.out.println(3);
            System.out.println(0/0); // 예외발생!!!
            System.out.println(4);   // 실행되지 않는다.
        } catch (ArithmeticException ae)    {
            ae.printStackTrace();
            System.out.println("예외메시지 : " + ae.getMessage());
        }    // try-catch의 끝

        System.out.println(6);
    }    // main메서드의 끝
}
```

### 사용자 정의 함수

- 예외처리를 선택적으로 하려고 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌고 있음

```java
class Ex8_11 {
    public static void main(String args[]) {
        try {
            startInstall();        // 프로그램 설치에 필요한 준비를 한다.
            copyFiles();        // 파일들을 복사한다. 
        } catch (SpaceException e)    {
            System.out.println("에러 메시지 : " + e.getMessage());
            e.printStackTrace();
            System.out.println("공간을 확보한 후에 다시 설치하시기 바랍니다.");
        } catch (MemoryException me)    {
            System.out.println("에러 메시지 : " + me.getMessage());
            me.printStackTrace();
            System.gc();         // Garbage Collection을 수행하여 메모리를 늘려준다.
            System.out.println("다시 설치를 시도하세요.");
        } finally {
            deleteTempFiles();        // 프로그램 설치에 사용된 임시파일들을 삭제한다.
        } // try의 끝
    } // main의 끝

   static void startInstall() throws SpaceException, MemoryException { 
        if(!enoughSpace())         // 충분한 설치 공간이 없으면...
            throw new SpaceException("설치할 공간이 부족합니다.");
        if (!enoughMemory())        // 충분한 메모리가 없으면...
            throw new MemoryException("메모리가 부족합니다.");
   } // startInstall메서드의 끝

   static void copyFiles() { /* 파일들을 복사하는 코드를 적는다. */ }
   static void deleteTempFiles() { /* 임시파일들을 삭제하는 코드를 적는다.*/ }

   static boolean enoughSpace()   {
        // 설치하는데 필요한 공간이 있는지 확인하는 코드를 적는다.
        return false;
   }

    static boolean enoughMemory() {
        // 설치하는데 필요한 메모리공간이 있는지 확인하는 코드를 적는다.
        return true;
   }
} // ExceptionTest클래스의 끝

class SpaceException extends Exception {
    SpaceException(String msg) {
       super(msg);    
   }
}

class MemoryException extends Exception {
    MemoryException(String msg) {
       super(msg);    
   }
}
```
