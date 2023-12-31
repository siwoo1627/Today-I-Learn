# 자바의 특징

- 운영체제에 독립적이다
  - 운영체제가 아닌 JVM과 통신하기 때문
- 객체지향 언어이다
- 자동 메모리 관리
- 네트워크와 분산처리 지원
- 멀티쓰레드 지원
- 동적 로딩 지원

# 리터럴

- 8진수는 `0`

- 16진수는 `0x`  
  
  ```java
  int octNUm = 010;
  int hexNum = 0x10;
  ```

- long은 뒤에 L붙임

- float는 뒤어 f붙임
  
  ```java
  long big = 100_000_000L;
  float pi = 3.14f;
  ```
  
  > 정수형 리터럴의 중간에 구분자 `_` 입력 가능

# 배열

- 배열은 `타입[] 변수 = new 타입[길이]{값};` 형태로 선언한다.
  
  ```java
  int[] score = new int[5];
  String[] hellow = new String[]{"안녕하세요"};
  String[] hellow = {"안녕하세요"};
  ```

- 배열의 모든 요소 출력은 `toString` 메서드 사용한다.
  
  ```java
  int[] iArr = {100, 95, 90};
  System.out.println(Arrays.toString(iArr));
  // [100, 95, 90];
  ```

- 2차원 배열은 `deepToString` 메서드를 사용한다.
  
  ```java
  int[][] arr2D = {{11,22},{33,44}};
  System.out.println(Arrays.deepToString(arr2D));
  // [[11,22],[33,44]]
  ```
