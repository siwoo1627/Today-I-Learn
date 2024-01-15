## 람다와 스트림(Lambda & stream)

### 람다식(Lambda Expression) 

: 메서드를 하나의 '식'으로 표현한 것이다, 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 '익명 함수(annoymous function)'라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random)*5)+1); // 1~5까지의 랜덤 값을 입력
```

- 메서드에서 이름과 반환 타입을 제거하고 매개변수 선언부와 몸통{ } 사이에 `->`를 추가하기만 하면 된다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/2917f8a4-ba42-4938-9bfe-4c392fca70d2)



### 메서드 참조

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/f3659d06-faad-4028-8a6e-47955f901852)

***

### 스트림(Stream)

: 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의

> 데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 됨 -> 코드의 재사용 성이 높아짐

특징

- 데이터 소스를 변경하지 않는다.

  - 데이터를 읽기만 할 뿐 데이터 소스를 변경하지 않는다.

- 일회용이다.

- 작업을 내부 반복으로 처리한다.

  - `stream.forEach(Systen.out::println);`

- 지연된 연산

  - 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다.

- `Stream<Integer>`와 IntStream

  - Stream<Integer>가 아닌 IntStream 등을 사용한다.

    > LongStream, DoubleStream 등

- 병렬 스트림

### 스트림의 연산

- 중간연산: 연산 결과가 스트림인 연산, 스트림에 연속해서 중간 연산할 수 있음
  - 중간 연산을 연속해서 연결할 수 있다.
- 최종 연산: 연산 결과가 스트림이 아닌 연산, 스트림의 요소를 소모하므로 단 한번만 가능
  - 단 한번만 연산이 가능하다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/e1ff5235-dbd3-436b-b1bc-96ecf714278c)
