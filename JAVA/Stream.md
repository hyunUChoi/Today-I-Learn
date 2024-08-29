# Stream
- 배열이나 리스트를 처리할때 주로 for문 또는 향상된 for문을 통해 처리하는데 코드 양이 많아지고 가독성이 떨어지는 것 같아 stream을 정리해보려고 한다.
<br>

## 간단한 특징
- ~~🕶 우선 멋있다~~
- for문과 Iterator를 사용하면 코드가 길어져 가독성과 재사용성이 떨어지고, 데이터 타입마다 다른 방식으로 다뤄야 하지만 스트림은 데이터 소스를 추상화하고, 자주 사용되는 메소드를 정의해 놓으면 소스에 상관없이 모두 같은 방식으로 다룰 수 있어 코드의 재사용성이 높아진다.
- 병렬 처리가 쉽다 -> 쓰레드를 이용해 많은 요소들을 빠르게 처리할 수 있다.
- 일회용이다 -> 한번 stream이 끝나면 재사용이 불가하다.
<br>

## Stream 생성하기
### Array 스트림
```
String[] arr = new String[]{"I", "want", "to", "go", "home"};
Stream<String> stream = Arrays.stream(arr);
```

### Collection(List, Collection, Set 등) 스트림
```
List<String> list = Arrays.asList("I", "want", "to", "go", "home");
Stream<String> stream = list.stream();
```
<br>

## Stream 가공하기(중간연산)
### filter
- 스트림 내 요소들을 하나씩 평가해서 걸러내는 작업 -> if문 역할
```
Arrays.stream(arr)/list.stream().filter(x -> x.equals("home"));
```

### map
- 스트림 내 요소들을 특정 값으로 변환 또는 타입을 변경할 때 사용
```
DummyVO[] dummy / List<DummyVO> dummy

Arrays.stream(dummy)/dummy.stream().map(x -> x.getDummyElement());
```

- 자매품 mapToInt(), mapToString() 등이 있다.
```
// mapping하려는 값이 int인 경우
Arrays.stream(dummy)/dummy.stream().mapToInt(Integer::intValue);

// mapping하려는 값이 int가 아닌 경우
Arrays.stream(dummy)/dummy.stream().mapToInt(Integer::parseInt);
```

### sorted
- 스트림 내 요소들 정렬 또는 Comparator
```
Arrays.stream(dummy)/dummy.stream().sorted();

// 오름차순
.sorted();

// 내림차순
.sorted(Comparator.reverseOrder());

// 특정값 기준 오름차순
.sorted(Comparator.comparing(DummyVO::getElement));

// 특정값 기준 내림차순
.sorted(Comparator.comparing(DummyVO::getElement).reversed());

// 이 밖에 compareTo 메소드를 사용해서 커스텀 정렬도 가능!
```

### distinct, skip, limit, peek, boxed
```
// 중복제거
.distinct()

// 앞에서부터 n개 무시하기
.skip(n)

// n개 까지만 요소 반환
.limit(n)

// 작업 특정 시점에서의 디버깅 용도
// 주의) peek는 중간연산(filter, map 등)에서는 작동하지 않고 최종연산(collect, 연산(count, sum 등) 등)이 있는 경우에만 동작한다
.peek()

// IntStream이나 DoubleStream과 같은 언박싱된 객체를 Stream 객체로 박싱해주는 용도
.boxed()
```
<br>

## Stearm 결과만들기(최종연산)
### 연산
- 스트림 요소들의 합, 평균, 최대값, 최소값, 카운팅 등의 연산
```
// 각 최종 연산 후에는 Optionnal 값으로 리턴되기 때문에 각 연산에 맞게 mapTo~~ 또는 .getAs~~() 등을 붙여줘야한다!!!
//합
.sum().getAsInt()

// 평균
.average().getAsDouble()

// 최대값
.max().getAsInt()

// 최소값
.min().getAsInt()

// 카운팅
.count()
```

### 출력
- 스트림 요소들을 출력할 때 보통 사용
```
Arrays.stream(dummy)/dummy.stream().forEach(n-> System.out.print(n + " ")) or .forEach(System.out::println)
```

### 소모
- 스트림 내 요소들을 하나씩 줄여가며 누적연산 수행
- reduce(초기값, (누적변수, 요소) -> 수행문)
```
Stream<String> stream = Stream.of("일","이","삼","사");
String result = stream.reduce("영",(a1,a2) ->  a1 + " + " + a2); // 영 + 일 + 이 + 삼 + 사
```

### 수집🌟🌟
- 스트림을 원하는 자료형으로 변환
```
// 배열변환
String[] arr = list.stream().toArray(String[]::new); // 이 뒤는 변형하고하는 타입으로 바꾸면 됨

// List, Set, Map 변환
Arrays.stream(arr).collect(Collectors.toList()); // 변환하시고 싶은 형태로 바꾸기만 하면 됨 -> toList() / toSet() / toMap()

// 요소 연결
list.stream().collect(Collectors.joining("?")); // 연결하고 싶은 문자 넣으면 됨
```
- 이 밖에 Matching, Finding, Grouping 등이 있지만 너무 길어질거 같아서 주로 많이 사용되는 것 위주로만 정리했다.
<br>

## 결론
👍 Stream은 Primitive Type을 다룰때는 일반적인 for문보다 느린 성능을 보이기 때문에 코드의 가독성을 높히고 코드의 양을 줄이는 것도 중요하지만 너무 남발하지 않도록 조심해야겠다!!<br>
~~하지만 간지나잖어 한잔 해~~
