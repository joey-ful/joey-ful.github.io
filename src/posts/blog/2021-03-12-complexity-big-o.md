---
title: '시간 복잡도(Big-O 표기법)과 공간복잡도'
category: "Algorithm"
date: "2021-03-14 12:00:00 +09:00"
desc: "codingtest, python heapq"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---


# 🦄 복잡도
시간 복잡도와 공간 복잡도는 반비례 관계

## 🦄 시간 복잡도 (Time Complexity)
- 알고리즘의 연산 횟수
- 코딩테스트 문제의 시간제한은 대략 1~5초
- Python이 초당 2000만번의 연산만 가능하다고 가정하는 것이 좋다
  - 5초에 1억번
- 10억번 이상 연산되지 않도록 하는 것이 좋다

### 빅오 표기법(Big-O Notation)
- 차수가 가장 큰 항만 고려한 시간 복잡도 표기법
```
3N³ + 5N² + 1,000,000
시간복잡도 = O(N³)
```
<img src="https://images.velog.io/images/jehjong/post/61fb9d3f-5f8f-471f-85bd-a4a15fe9c19c/image.png" width="500"/>


- [Big-O Notation Cheat Sheet](https://www.bigocheatsheet.com/)
![image](https://user-images.githubusercontent.com/52592748/110871730-411cf980-8312-11eb-92f9-234f497e90b6.png)


### 연산 횟수에 따른 시간 복잡도
#### 연산 횟수가 5억
- C언어 - 1~3초
- Python - 5~15초
  - PyPy는 때로 C보다 빠르다 (대신 공간복잡도가 늘어날 수 있다)

#### O(N³), N=5000
- 연산 횟수가 1250억
   - Python은 약 2500초

#### 시간제한에 따른 알고리즘 설계
|N의 범위| 권장 시간복잡도 |
|-------|-------|
|500 (5백)|	O(N³)|
|2,000 (2천)|O(N²)|
|100,000 (10만)|	O(NlogN)|
|10,000,000 (천만)|O(N)|

#### 수행 시간 측정 코드
```python
import time
start_time = time.time()	# 측정 시작

# 프로그램 소스코드
end_time = time.time()		# 측정 종료
print('time:', end_time - start_time)	# 수행 시간 출력
```


## 🦄 공간 복잡도 (Space Complexity)
메모리의 양
메가바이트 단위로 표기
```
int a[1000]: 4KB
int a[1000000]: 4MB
int a[2000][2000]: 16MB
```


