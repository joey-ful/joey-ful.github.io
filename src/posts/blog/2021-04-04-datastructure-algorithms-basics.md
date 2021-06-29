---
icon: "🗃"
title: 'Data Structures and Algorithms basics'
category: "Algorithm"
date: "2021-04-04 12:00:00 +09:00"
desc: "Jay Wengrow의 A Common-Sense Guide to Data Structures and Algorithms"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# 자료구조
- 읽기 Read - 특정 인덱스의 값을 확인하는 것
- 검색 Search - 특정 값의 인덱스를 확인하는 것
- 삽입 Insert
- 삭제 Delete

# 배열 Array
- 배열 첫 항목의 메모리 주소를 기억해두기 때문에 특정 인덱스의 원소에 한번에 접근 가능

## Big-O
- 읽기 Read - O(1)
- 검색 Search - O(N) 뒤에 있을 수록 찾기 힘듬
- 삽입 Insert - O(N) 뒤에 삽입하면 O(1), 최악은 맨 앞에 삽입 O(N + 1)
- 삭제 Delete - O(N) 뒤를 삭제하면 O(1), 최악은 맨 앞을 삭제 O(N + 1)

## Set
- 중복되는 데이터가 없음을 알려주는 자료구조
- 읽기와 검색은 배열과 같다
- 삽입할 항목이 있는지 없는지 확인해야하기 때문에 모든 삽입은 먼저 검색 과정을 거친다
  - 마지막에 삽입하면 N + 1, 맨 앞에 삽입하면 2N + 1

## Ordered Array
- 항상 순서를 유지하는 배열
- 데이터 삽입시 항상 비교 과정을 거친다
  - 앞쪽에 삽입하면 비교를 적게 하고 대신 삽입 과정이 길다
  - 뒤쪽에 삽입하면 비교를 많이 하고 대신 삽입 과정이 짧다
- 삽입 O(N)
- 삭제 O(1)
- Linear Search는 오래 걸리지만 Binary Search 를 사용하면 O(logN)
  - Ordered Array는 삽입은 느리지만 대신 검색이 매우 빠르다

# 연결 리스트 Linked List
- 구조체와 포인터를 사용해 구현
- 배열과는 달리 데이터가 메모리에 동적으로 불연속적으로 할당된다
- 노드에 자료와 다음 노드의 메모리 주소를 저장해 연결
  - 첫 노드는 head, 마지막 노드는 tail
- head만 바로 접근이 가능

## Big-O
- Read - head는 O(1) tail은 O(N)
- Search - head는 O(1) tail은 O(N)
- Insert - head는 O(1) tail은 O(N + 1)
- Delete - head는 O(1) tail은 O(N + 1)

## Array vs Linked List

|특징|배열|연결리스트|
|---|---|-------|
|항목 접근|특정한 인덱스에 위치한 항목에 바로 접근 가능|head만 바로 접근 가능|
|메모리 할당|데이터가 들어갈 공간을 미리 한번에 연속적으로 할당|필요할 때마다 아무데나 할당 (동적 할당)|
|삽입/삭제|삽입/삭제 후 항목들을 shift 해야하는 번거로움|해당위치까지 탐색만 했으면 O(1)으로 삽입/삭제 가능|
|사용 예시|특정한 위치의 항목에 접근이 필요한 경우|한 리스트에서 삽입/삭제가 연속적으로 이루어지는 경우|
|Reading|O(1)|O(N)|
|Search|O(N)|O(N)|
|Insertion|O(N) (맨뒤는 O(1))|O(N) (맨앞은 O(1))|
|Deletion|O(N) (맨뒤는 O(1))|O(N) (맨앞은 O(1))|

- 사실 이렇게 비교하면 연결리스트가 더 좋은게 없어보인다
- 하지만 하나의 리스트에서 여러번의 삭제나 삽입이 필요한 경우 연결리스트의 장점이 나타난다
  - 배열의 경우 탐색 + 삽입/삭제 후 항목들의 자리이동을 하는 shift과정이 필요하지만
  - 연결리스트의 경우 탐색 + O(1)의 삽입/삭제만 있으면 된다
- e.g. 1000개의 메일 주소가 있고 그 중 100개의 스팸메일을 지우는 작업


### 양방향 연결 리스트 Doubly Linked List
- 양방향 연결 리스트는 이전 노드와 다음 노드의 메모리 주소를 동시에 갖고 있다
- 따라서 첫 노드와 마지막 노드의 메모리 주소를 기억하고 있어 맨앞과 맨뒤 항목에 바로 접근이 가능하다
- 큐를 구현하기에 좋은 자료구조

## 큐 Queue
- 추상적 자료구조 - 이론적인 규칙들의 모음으로 다른 자료구조를 사용해 구현하는 형태의 자료구조
### 특징
- FIFO (First-in First-out)
- 삽입시 맨뒤에 추가
- 삭제시 맨앞을 삭제
- 맨앞만 읽을 수 있음

```python
class Node:
  def __init__(self, data):
    self.data = data

class DoublyLinkedList
  def __init__(self, )
```

## Stack 스택
- 추상적 자료구조
### 특징
- LIFO (Last-in First-out)
- 삽입시 맨뒤에 추가
- 삭제시 맨뒤를 삭제
- 맨뒤만 읽을 수 있음

### 활용예시
- 웹 브라우저 방문기록 (뒤로가기) - 가장 나중에 열린 페이지부터 보여주기
- 실행 취소 (undo) - 가장 나중에 실행된 것부터 실행 취소

## Dequq 데크 Double Ended Queue
- 양끝에 삽입과 삭제가 가능

# Tree
- 사이클이 존재하지 않고 모든 노드들이 연결된 그래프
- 노드로 이루어진 자료구조로 하나의 노드가 여러개의 노드에 연결되어 있을 수 있다
- 레벨, 부모와 자식, 균형

## Tree Traversal
- Inorder - 왼쪽부터 오른쪽으로 방문
- Preorder - 부모 -> 왼쪽 -> 오른쪽 순으로 방문 (부모를 방문했으면 그 다음으로 무조건 왼쪽 자식을 방문해야함)
- Postorder - 왼쪽 -> 오른쪽 -> 부모 순으로 방문

## Binary Tree
- 노드가 최대 두 개인 트리

### Complete, Perfect, Full
- Complete - 구멍이 없는 것. 모든 노드가 왼쪽부터 쌓여있는 것 (오른쪽 노드가 비어있을 수 있음)
- Perfect - 모든 자식이 차 있는 것. 모든 레벨이 꽉 차 있는 것.
- Full - 부모가 두 개의 자식이 있거나 아예 자식이 없는 경우

## Binary Search Tree
- 각 노드는 최대 2개의 노드를 가질 수 있다
- 왼쪽 자식 < 부모 < 오른쪽 자식
- 삽입/삭제 후에도 항상 순서를 유지하고 검색이 용이한 자료구조

## Big-O
- Search - O(log N)
  - 크면 오른쪽 작으면 왼쪽
- Insertion - O(log N)
  - 검색보다 하나의 스텝이 더 필요
  - 완전히 불균형할 경우 O(N)
- Deletion - O(log N)
  - 검색 후 삭제 과정이 추가
  - 자식이 없는 노드면 바로 삭제
  - 자식이 있는 노드면 해당 노드보다 큰값을 가진 노드 중 가장 작은 것으로 교체 (successor node)

# Binary Heap
- weakly ordered하다
- max heap의 heap condition: 모든 부모 노드는 자손 노드들보다 값이 커야한다
- 트리가 무조건 complete해야한다
- 배열의 자료가 많으면 마지막 노드 접근이 오래걸린다는 문제를 해결하기 위해 배열로 구현한다
  - 각 노드에 인덱스를 부여
  - 마지막 노드는 항상 마지막 인덱스

## Big-O
- 삽입 O(log N)
- 삭제 O(log N)

## Traversing
- 왼쪽 자식 인덱스 = (부모의 인덱스 * 2) + 1
- 오른쪽 자식 인덱스 = (부모의 인덱스 * 2) + 2
- 부모의 인덱스 = (자식의 인덱스 - 1) / 2

## Priority Queue
- 삭제와 접근은 일반 큐와 같지만 삽입은 정렬 리스트 ordered array 와 같은 자료구조
- 맨앞만 삭제 가능
- 맨앞만 읽기 가능
- 자료를 삽입해도 항상 순서 유지
  - e.g. 병동에서 부상의 정도에 따라 진료 순서를 정해놓는 경우 (triage system)

### Priority Queue를 Heap으로
- heap은 priority queue에 적합한 자료구조
- 가장 우선순위가 높은 항목에 바로 접근 가능
- 삽입과 삭제가 O(log N) 이라는 빠른 시간에 가능

### Priority Queue Big-O
- 삭제 O(1), 삽입 O(N)
- 자료가 많은 경우 마지막 노드 접근이 너무 오래 걸리는 문제 발생
  - heap으로 구현하면 문제 해결

# BST vs Heap

|자료구조|Ordered Array|Binary Search Tree|Heap|
|------|----|------|---------|
|Search|O(log N)|O(log N)|O(N)|
|Insertion|O(N)|O(log N)|O(log N)|
|Deletion|O(1)|O(log N)|O(log N)|

- Heap은 가장 우선순위가 높은 항목에 대해서만 탐색이 O(1)
- Heap은 priority queue에 적합한 자료구조
- BST는 Heap과 다르게 항상 completely balanced되어있지 않기 때문에 최악의 경우 탐색이 O(N)

# 정렬 Sort

|Sort|Best Case|Average Case|Worst Case|공간 복잡도|stability|방법|
|---|--------|-------|-|-----|---------------|-|
|Bubble Sort|O(N)|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|stable|둘씩 비교|
|Insertion Sort|O(N)|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|stable|왼쪽들과 비교|
|Merge Sort|O(N log N)|O(N log N)|O(N log N)|O(N)|stable|둘씩 묶어서 정렬|
|Selection Sort|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|unstable|매단계 최솟값 탐색|
|Heap Sort|O(N log N)|O(N log N)|O(N log N)|O(1)|unstable|힙에 모두 삽입 후 모두 삭제|
|Quick Sort|O(N log N)|O(N log N)|O(N<sup>2</sup>)|O(N log N)|unstable|pivot과 left pointer, right pointer|

## 버블 정렬 Bubble Sort
- 나란히 있는 항목 둘씩 비교
- 순서가 바뀌어있으면 스왑으로 자리 바꿈
- 모든 숫자들을 끝까지 비교했으면 다시 처음부터 반복
- O(N<sup>2</sup>)

## 삽입 정렬 Insertion Sort
- 인덱스 1부터 항목을 선택
- 선택한 항목과 그 항목의 왼쪽 항목들을 비교
- 선택한 항목이 더 작으면 스왑 (왼쪽 항목들 중 선택항목보다 크면서 가장 작은 것과 스왑)
- 다음 인덱스 항목을 선택해서 또 반복
- O(N<sup>2</sup>)
  - Best Case - O(N) - 이미 원하는대로 정렬된 경우
  - Average Case - O(N<sup>2</sup>/2)
  - Worst Case - O(N<sup>2</sup>) - 반대로 정렬된 경우 매 단계마다 스왑

## 병합 정렬 Merge Sort
- 항목들을 둘씩 묶어 각 묶음 내에서 정렬
- 묶음을 또 두개씩 묶어 하나의 묶음 내에서 정렬
- 시간복잡도 O(log N)
- 공간복잡도 O(N)

## 선택 정렬 Selection Sort
- 매 단계 최솟값과 첫 항목의 위치를 바꾼다
- 스왑 후 다음 항목으로 넘어간다
- 매 단계 0번이나 한번의 스왑을 진행
- O(N<sup>2</sup>) - 버블 정렬보다 두 배 가량 빠르지만 그래도 O(N<sup>2</sup>)
  - Best Case - O(N<sup>2</sup>/2) - 이미 원하는대로 정렬된 경우
  - Average Case - O(N<sup>2</sup>/2)
  - Worst Case - O(N<sup>2</sup>/2) - 반대로 정렬된 경우 모두 비교 후 전부 자리이동

## 힙 정렬 Heap Sort
- 하나씩 힙에 삽입한 후 가장 큰 것부터 차례대로 삭제해서 정렬 (max heap)
- O(N log N)

## 퀵 정렬 Quick Sort
- partition과 재귀를 사용한 정렬
- O(N log N)
  - 매 단계 반으로 쪼개져서 logN 단계
  - 각 단계마다 N번 비교
  - Best Case - O(N log N)
  - Average Case - O(N log N)
  - Worst Case - O(N<sup>2</sup>) - pivot이 항상 끝에 위치

### Partition
- pivot을 하나 골라 pivot보다 작은 값들은 pivot의 왼쪽, 큰 값들은 pivot의 오른쪽에 위치하게 하는 것

### Quick Select
- 정렬은 필요없지만 특정 순위의 값을 알고싶은 경우 사용
- Quick 정렬과 Binary Search 사용
- O(N)

# Stable vs Unstable
- 같은 값을 가진 항목들의 순서를 유지하는지 안 하는지
  - Stable - 같은 값을 가진 항목들의 순서가 정렬 후에도 유지되는 것
  - Unstable - 같은 값을 가진 항목들의 순서가 정렬 후에도 유지되지 않는 것
- 같은 값을 가진 항목들을 구분해야할 때 stable한 정렬이 필요
- e.g. 학생 이름과 성적이 있을 때 성적순으로 정렬. 하지만 기존 이름 순서는 보존하고 싶은 경우 stable한 정렬을 사용해야 한다 [그림 출처](https://www.freecodecamp.org/news/stability-in-sorting-algorithms-a-treatment-of-equality-fa3140a5a539/)
  ![image](https://user-images.githubusercontent.com/52592748/113816883-6cc7be00-97b0-11eb-9755-776d4bab28c8.png)
  ![image](https://user-images.githubusercontent.com/52592748/113816896-705b4500-97b0-11eb-94ec-b00b003e4263.png)

## Stable Sort
- 버블 정렬 Bubble Sort
- 삽입 정렬 Insertion Sort
- 병합 정렬 Merge Sort

## Unstable Sort
- 선택 정렬 Selection Sort
- 퀵 정렬 Quick Sort
- 힙 정렬 Heap Sort

# Hash Table
- hash function으로 키를 인덱스로 변경
- 검색: O(1)
- hash function에 다른 인풋을 넣었는데 같은 아웃풋이 나오면 collision이 발생

## Collsion 해결법
- Open Addressing과 Separate Chaining 모두 O(m)
- Open Addressing은 연속된 공간에 저장해서 캐시 효율이 높다 (데이터 개수가 적으면 효율이 굳)
- Separate Chaining은 테이블의 확장을 늦출 수 있다

### 1. Open Addressing (개방주소법)
- 충돌 발생시 다른 해시 버킷 (다른 위치)에 저장
- 해시 버킷이 많이 차있을수록 Worst Case발생 빈도가 높아진다
  - Worst Case: 비어있는 버킷을 못 찾아 탐색시작위치에 돌아오는 것

1. Linear Probing - 순차적으로 탐색
2. Quadratic Probing - 2차 함수를 이용해 탐색 (키를 i^2순으로 증가)
3. Double Hashing Probing - 다른 해시 함수를 이용 (연산이 많이 필요해짐)

### 2. Separate Chaining (분리 연결법)
- Open Addressing보다 빠르다
- 데이터가 적으면(키-값 쌍이 6~7개까지) 연결리스트를 사용하는 것이 오버헤드가 적다 트리는 메모리 사용량이 많기 때문

1. Linked List - 버킷들을 연결리스트로 만든다. Collsion 발생시 해당 버킷의 리스트에 추가
2. Red-Black Tree - 버킷들을 트리에 저장

# Trie
- autocomplete와 같은 기능을 위해 단어를 저장하는데 최적
- 다른 노드들을 가리키는 노드들의 집합
- 자식 노드 개수의 제한이 없음

## Big-O
- 검색: O(K) 
  - K: 문자열의 길이, 속도는 단어의 수 N에 비례

## 구현
- 노드 클래스 - 각 노드는 해시 테이블을 지님 (키: 자식의 알파벳, 값: 트라이 노드 인스턴스)
  - 단어의 끝을 나타내기 위해 값으로 * 을 저장
- 트라이 클래스 - 루트 노드 값을 저장하고 트라이 메소드를 지님

```python
class TrieNode:
  def __init__(self):
    self.children = {}

class Trie:
  def __init__(self):
    self.root = TrieNode()
```

## 검색 Search
- 어떤 문자열이 트라이내 단어의 prefix인지
1. currentNode 생성 후 루트로 초기화
2. 문자열의 문자마다 반복문
3. currentNode가 해당 문자를 자식으로 갖는지 확인
4. 안 가지면 return None
5. 가지면 currentNode를 자식으로 업데이트 후 다음 문자로 넘어감
6. 문자열의 끝에 도달했다면 return currentNode

```python
def search(self, word):
  currentNode = self.root

  for char in word:
    if currentNode.children.get(char):
      currentNode = currentNode.children[char]
    else:
      return None

  return currentNode
```

## 삽입 Insert
1. currentNode 생성 후 루트로 초기화
2. 문자열의 문자마다 반복문
3. currentNode가 해당 문자를 자식으로 갖는지 확인
4. 가지면 currentNode를 자식으로 업데이트 후 다음 문자로 넘어감
5. 안 가지면 currentNode가 해당 자식을 갖도록 하고 currentNode를 새로운 자식으로 업데이트 후 다음 문자로 넘어감
6. 마지막 문자를 삽입 후 *을 자식으로 삽입해 문자열이 끝났음을 알림

```python
def insert(self, word):
  currentNode = self.root

  for char in word:
    if currentNode.children.get(char):
      currentNode = currentNode.children[char]
    else:
      currentNode.children[char] = TrieNode()
      currentNode = currentNode.children[char]
  
  currentNode.children["*"] = None

```

# 그래프 Graph
- 데이터들이 어떻게 연결되어있는지, 관계에 최적화된 자료구조
- 정점 (vertices)와 간선 (edges)로 이루어짐
- **connected graph** - 모든 노드가 직간접적으로 연결
- **directed graph** - 관계가 단방향
- **undirected graph** - 양방향
- **spanning tree** - 정점들이 최소한의 간선으로 연결된 그래프

## 그래프 표현방법
- Adjacency List - n번 노드와 연결된 모든 노드들을 배열의 n번째에 배열로 넣는다
  - e.g. 1번 노드와 연결된 노드들이 2, 3, 5번 노드라면 배열의 1번째 인덱스의 배열에 2, 3, 5를 삽입
  ```python
  [
    [],
    [2, 3, 5],
    [],
    ...
  ]
  ```

- Adjacency Matrix - `adj[i][j]`에 i번 노드부터 j번 노드까지 방향이 존재하면 1, 없으면 0 삽입

## 검색 Search
- 특정 정점을 찾거나 하나의 정점부터 다른 정점까지 지나가는 목적으로 검색
- 그래프 탐색의 핵심은 지나간 정점을 기록하는 것 (사이클을 가질 수 있기에 무한 사이클 방지)

### Depth-First Search DFS
- 재귀를 사용한 탐색 방법
1. 랜덤한 정점에서 시작
2. 현재 정점을 visited 해시에 저장
3. adjacent한 정점들 방문
4. 이미 방문한 곳이면 무시
5. 처음 방문하는 곳이면 그 정점의 adjacent한 정점들 방문

### Breadth-First Search BFS
- 큐를 사용한 탐색 방법
1. 랜덤한 정점에서 시작
2. 시작 정점을 visited 해시에 저장
3. 시작 정점을 queue에 저장
4. queue가 비어있지 않은 동안 반복문 실행
5. queue의 첫 정점을 제거 후 현재 정점으로 설정
6. 현재 정점의 adjacent한 정점들 방문
7. 이미 방문한 곳이면 무시
8. 처음 방문하는 곳이면 visited에 기록 후 queue에 추가
9. 4번부터 반복

## Big-O
O(V + E)

## Dijkstra's Algorithm
- Weighted graph에서 Shortest Path Problem 해결을 위한 방법 중 하나
- O(V<sup>2</sup>)
- 
  - visited hash table
  - (cost/distance, adjacent vertex) queue
  - cost/distance hash table


# 동적 프로그래밍 Dynamic Programming
- 재귀함수를 사용하는 경우 overlapping subproblems를 해결하는 방식
  - 한 문제를 해결하기 위해 해결해야하는 더 작은 문제를 subproblem이라 한다
  - 이 때, 똑같은 subproblem을 반복적으로 풀어야한다면 overlapping subproblems이 발생
  - O(2<sup>N</sup>)

## Memoization
- 이전에 실행한 값을 기억해 반복작업을 제한하는 방식
- 피보나치 수열을 구하는 문제의 경우 해시 테이블에 계산한 값들을 저장해둔다
- O(N)

## Bottom-Up
- 반복문으로 푸는 방식
- O(N)
- 피보나치 예시: 0, 1부터 계산을 시작
```python
a = 0
b = 1

for _ in range(1, n):
  temp = a
  a = b
  b = temp + a
```