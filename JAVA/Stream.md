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

### distinct, skip, limit, peek
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
```
<br>

## Stearm 결과만들기(최종연산)
