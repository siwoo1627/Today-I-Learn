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

### Optional<T>

: "'T'타입의 객체"를 감싸는 래퍼 클래스이다.

- Optional타입의 객체에는 모든 타입의 객체를 담을 수 있다.

```java
String str = "str";
Optional<String> opval = Optional.of(str);
Optional<String> opval = Optional.of("abc");
Optional<String> opval = Optional.of(new String("abc"));
Optional<String> opval = Optional.ofNullalble(str); // null 입력 가능
Optional<String> opval = Optional.<String>empty(); // 빈 객체로 초기화
```

reduce()

```java
import java.util.*;
import java.util.stream.*;

class Ex14_9 {
	public static void main(String[] args) {
		String[] strArr = {
			"Inheritance", "Java", "Lambda", "stream",
			"OptionalDouble", "IntStream", "count", "sum"
		};

		Stream.of(strArr).forEach(System.out::println);

		boolean noEmptyStr = Stream.of(strArr).noneMatch(s->s.length()==0);
		Optional<String> sWord = Stream.of(strArr)
					               .filter(s->s.charAt(0)=='s').findFirst();

		System.out.println("noEmptyStr="+noEmptyStr); // true
		System.out.println("sWord="+ sWord.get()); // stream

		// Stream<String>을 IntStream으로 변환
		IntStream intStream1 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream2 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream3 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream4 = Stream.of(strArr).mapToInt(String::length);

		int count = intStream1.reduce(0, (a,b) -> a + 1); // reduce를 사용하여 스트림의 요소 수를 계산
		int sum   = intStream2.reduce(0, (a,b) -> a + b); // reduce를 사용하여 스트림의 모든 요소의 합을 계산
		// (a, b) -> a + b는 "누적된 결과(a)를 가져와서 다음 요소(b)를 더하고, 그 합을 새로운 누적 결과로 반환한다
        // 11 + 4 + 6 + 6 + 14 + 9 + 5 + 3 = 58
		OptionalInt max = intStream3.reduce(Integer::max);
		OptionalInt min = intStream4.reduce(Integer::min);
		System.out.println("count="+count); // 8
		System.out.println("sum="+sum); // 58
		System.out.println("max="+ max.getAsInt()); // 14
		System.out.println("min="+ min.getAsInt()); // 3
	}
}

```

collect() : 스트림의 최종연산, 매개변수로 컬렉터를 필요로 한다.

Collector : 인터페이스, 컬렉터는 이 인터페이스를 구현해야 한다.

Collectors : 클래스, static메서드로 미리 작성된 컬렉터를 제공한다.

![image](https://github.com/siwoo1627/Today-I-Learn/assets/114638386/491374c5-d45d-411b-8b56-6a8ebfc8b9c9)

> 스트림을 컬렉션(List, Map,Set 등)으로 변경하는듯

````java
List<String> names = stuStream.map(Student::getName).collect(Collectors.toList());
````

- 그룹화[groupBy()]: 스트림의 요소를 특정 기준으로 그룹화하는 것을 의미
  - 스트림을 2개 이상의 그룹으로 나눌 때
- 분할[partitioningBy()]: 스트림의 요소를 두 가지, 지정된 조건에 일치하는 그룹과 일치하지 않는 그룹으로의 분할을 의미
  - 스트림을 2개로 나눌 때 사용하면 빠름
