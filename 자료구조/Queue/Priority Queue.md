# Priority Queue

### Priority Queue란
우선순위 큐로써 <strong><em>일반적인 큐의 구조 FIFO(First In First Out)를 가지면서</em></strong>, 데이터가 들어온 순서대로 데이터가 나가는 것이 아닌  
<strong><em>우선순위를 먼저 결정하고 그 우선순위가 높은 데이터가 먼저 나가는 자료구조</em></strong>
<br><br>

### 진행방식
데이터를 삽입할 때 우선순위를 기준으로 최대 힙 혹은 최소 힙을 구성하고 데이터를 꺼낼 때 루트 노드를 얻어낸 뒤  
루트 노드를 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입한 후 아래로 내려가면서 적절한 자리를 찾아 옮기는 방식으로 진행
<br><br>

### 특징
1. 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조 -> 우선순위 큐에 들어가는 원소는 비교가 가능한 기준이 있어야함
2. 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어짐
<br><br>

### 사용방법
```
// 낮은 숫자가 우선순위가 높은 방식
PriorityQueue<Integer> pq = new PriorityQueue<>();

// 높은 숫자가 우선순위가 높은 방식
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

// 이중 배열에서 0번째 낮은 숫자가 우선순위가 높은 방식(람다식)
PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[0] - o2[0]);

// 이중 배열에서 0번째 낮은 숫자가 우선순위가 높은 방식(Comparator)
PriorityQueue<int[]> pq = new PriorityQueue<>(new Comparator<int[]>() {
    @Override
    public int compare(int[] o1, int[] o2) {
        return o1[0] - o2[0];
    };
});
```

### Queue에 추가/삭제/검색
||예외발생|값 리턴|
|------|------|------|
|추가|add() (Queue에 값 추가 가득찬 경우 IllegalStateException)|offer() (Queue에 값 추가 가득찬 경우 false)|
|삭제|remove() (첫번째 값 제거, 비어있으면 NoSuchElementException)|poll() (첫번째 값 제거, 비어있으면 null)|
|검색|element() (첫번째 값 반환, 비어있으면 NoSuchElementException)|peek() (첫번째 값 반환, 비어있으면 null)|
* clear() : Queue 비우기
<br><br>

### Queue 값 추가 방식
![image](https://github.com/hyunUChoi/Today-I-Learn/assets/103620466/20400fb1-0779-42d0-8036-9e43fdf691d2)
<br><br>

### Queue 값 삭제 방식
![image](https://github.com/hyunUChoi/Today-I-Learn/assets/103620466/8b724986-c791-4020-895c-3583c0e022f9)

사진 출처 : https://haservi.github.io/posts/algorithms/priority-queue/
