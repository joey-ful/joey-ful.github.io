---
icon: "📑"
title: '[CheatSheet] 파이썬 Python 코딩테스트'
category: "Algorithm"
date: "2021-03-20 12:00:00 +09:00"
desc: "cheatsheet"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

## 복사, 참조
- 배열이 변경되어도 같은 객체로 저장
- 문자열이 변경되면 복사가 일어남

## index
리스트에서 특정 값의 인덱스 찾기
```python
a = [1, 2, 3, 4]
print(a.index(2))
# 1
```

## 거꾸로 iterate
```python
numbers = [1, 2, 3, 4]
length = len(numbers)
for i in range(length):
	print(numbers[length - i])
# 4
# 3
# 2
# 1
```

## 첫 인덱스를 시작으로 거꾸로 iterate
[프로그래머스 탐욕법에서 조이스틱 문제](https://programmers.co.kr/learn/courses/30/lessons/42860)
- AAA에서 키보드 상하좌우 키로 이름을 정할 때 만약 CAT을 만들고싶을 때
  - 인덱스 0에서 C 만들기
  - 인덱스 2로 가서 T 만들기

```python
numbers = [1, 2, 3, 4]
length = len(numbers)
for i in range(length):
	print(numbers[-i])
# 1
# 4
# 3
# 2
```

## enumerate
인덱스와 값을 바로 가져올 수 있다 (단, iterable 해야함)
```python
string = hello
for i, v in enumerate(string):
	print(i, v)
# 0 h
# 1 e
# 2 l
# 3 l
# 4 o
```

## ascii: ord(), chr()
파이썬에서는 C처럼 문자에 숫자를 더해 새로운 문자를 구할 수 없다
즉, 아래 식이 성립하지 않는다
```python
'A' + 32 = 'a'
```

숫자는 숫자끼리 연산해야하기 때문에 ord() 함수로 아스키 숫자로 바꾼 후
그 숫자를 chr() 함수로 문자화해줘야한다
```python
ord('A')
#65
chr(ord('A') + 32)
#a
```

## dictionary get()
CODE라는 사전에서 키가 i 인 것의 값을 가져온다
i는 s의 항목이다

```python
CODE.get(i) for i in s
```
```python
CODE = {'a': '.-',     'b': '-...',   'c': '-.-.', 
                'd': '-..',    'e': '.',      'f': '..-.',
                'g': '--.',    'h': '....',   'i': '..',
                'j': '.---',   'k': '-.-',    'l': '.-..',
                'm': '--',     'n': '-.',     'o': '---',
                'p': '.--.',   'q': '--.-',   'r': '.-.',
                's': '...',    't': '-',      'u': '..-',
                'v': '...-',   'w': '.--',    'x': '-..-',
                'y': '-.--',   'z': '--..',
                }

s = "hello"

print(''.join(CODE.get(i) for i in s))
#......-...-..---
```


## zip()
두 iterable을 하나의 튜플로 합치기
`*` 를 사용하면 튜플로 분리 가능
```python
coordinate = ['x', 'y', 'z']
value = [3, 4, 5]

result = zip(coordinate, value)
result_list = list(result)
print(result_list)
# [('x', 3), ('y', 4), ('z', 5)]

c, v =  zip(*result_list)
print('c =', c)
print('v =', v)
c = ('x', 'y', 'z')
v = (3, 4, 5)
```

## [::-1]
배열을 똑같이 복사하되 반대 순서로
```python
arr = [1, 2, 3, 4]
print(arr[::-1])
#[4, 3, 2, 1]
```


## 90도 회전 with zip
```python
arr = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
      ]
arr[::-1]
# [	
#	[7, 8, 9],
#	[4, 5, 6],
#   	[1, 2, 3]
#  ]
  
tup = zip(*arr[::-1])
#(7, 4, 1)
#(8, 5, 2)
#(9, 6, 3)

print([list(elem) for elem in tup])
# [	
#	[7, 4, 1],
#	[8, 5, 2],
#   	[9, 6, 3]
#  ]
```

## 90도 회전
```python
arr = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
      ]
new = [row[:] for row in arr]
m = len(arr[0])

for x in range(0,m):
	for j in range(0,m):
		new[j][m-1-x] = arr[x][j]
print(new)
# [	
#	[7, 4, 1],
#	[8, 5, 2],
#   	[9, 6, 3]
#  ]
```


## [:n]
n번째까지

## [n:]
n번째부터

## map(function, iterable)
```python
a = '123'

# a의 항목들에 int() 적용 convert to int
b = map(int, a)		#object
print(set(b))
# {1, 2, 3}
```

## sum(iterable, start)
List 항목들의 합 구하기
```python
a = [1, 2, 3, 4, 0, 2]

print(sum(a))
#12
```

## map과 sum
문자열 내 문자들의 합 구하기
```python
a = '123402'

sum(map(int, a[:3]))
# 6
```
- `a[:3]` 은 '123'
- map으로 '123'의 항목마다 int() 적용
- sum으로 모두 int가 된 '123'의 각 항목들 더함
- `1 + 2 + 3 = 6`


## 삼항연산자
```python
A if 조건 else B
```

```python
numbers = [1, 3, 5, 7]
for i in range(len(numbers)):
	print(numbers[i], 'is smaller than 4') if numbers[i] < 4 else print(numbers[i], 'is bigger than 4')
# 1 is smaller than 4
# 3 is smaller than 4
# 5 is bigger than 4
# 7 is bigger than 4
```

- 값을 지정할 때는
```python
A = a if 조건 else b
```

```python
num = 5
type = 'odd' if num % 2 == 1 else 'even'
# odd
```

## str to list 변환
```python
text = 'hello'
a = list(text)
print(a)
# ['h', 'e', 'l', 'l', 'o']
```