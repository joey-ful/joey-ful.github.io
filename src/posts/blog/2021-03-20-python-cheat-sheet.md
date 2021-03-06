---
icon: "๐"
title: '[CheatSheet] ํ์ด์ฌ Python ์ฝ๋ฉํ์คํธ'
category: "Algorithm"
date: "2021-03-20 12:00:00 +09:00"
desc: "cheatsheet"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

## ๋ณต์ฌ, ์ฐธ์กฐ
- ๋ฐฐ์ด์ด ๋ณ๊ฒฝ๋์ด๋ ๊ฐ์ ๊ฐ์ฒด๋ก ์ ์ฅ
- ๋ฌธ์์ด์ด ๋ณ๊ฒฝ๋๋ฉด ๋ณต์ฌ๊ฐ ์ผ์ด๋จ

## index
๋ฆฌ์คํธ์์ ํน์  ๊ฐ์ ์ธ๋ฑ์ค ์ฐพ๊ธฐ
```python
a = [1, 2, 3, 4]
print(a.index(2))
# 1
```

## ๊ฑฐ๊พธ๋ก iterate
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

## ์ฒซ ์ธ๋ฑ์ค๋ฅผ ์์์ผ๋ก ๊ฑฐ๊พธ๋ก iterate
[ํ๋ก๊ทธ๋๋จธ์ค ํ์๋ฒ์์ ์กฐ์ด์คํฑ ๋ฌธ์ ](https://programmers.co.kr/learn/courses/30/lessons/42860)
- AAA์์ ํค๋ณด๋ ์ํ์ข์ฐ ํค๋ก ์ด๋ฆ์ ์ ํ  ๋ ๋ง์ฝ CAT์ ๋ง๋ค๊ณ ์ถ์ ๋
  - ์ธ๋ฑ์ค 0์์ C ๋ง๋ค๊ธฐ
  - ์ธ๋ฑ์ค 2๋ก ๊ฐ์ T ๋ง๋ค๊ธฐ

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
์ธ๋ฑ์ค์ ๊ฐ์ ๋ฐ๋ก ๊ฐ์ ธ์ฌ ์ ์๋ค (๋จ, iterable ํด์ผํจ)
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
ํ์ด์ฌ์์๋ C์ฒ๋ผ ๋ฌธ์์ ์ซ์๋ฅผ ๋ํด ์๋ก์ด ๋ฌธ์๋ฅผ ๊ตฌํ  ์ ์๋ค
์ฆ, ์๋ ์์ด ์ฑ๋ฆฝํ์ง ์๋๋ค
```python
'A' + 32 = 'a'
```

์ซ์๋ ์ซ์๋ผ๋ฆฌ ์ฐ์ฐํด์ผํ๊ธฐ ๋๋ฌธ์ ord() ํจ์๋ก ์์คํค ์ซ์๋ก ๋ฐ๊พผ ํ
๊ทธ ์ซ์๋ฅผ chr() ํจ์๋ก ๋ฌธ์ํํด์ค์ผํ๋ค
```python
ord('A')
#65
chr(ord('A') + 32)
#a
```

## dictionary get()
CODE๋ผ๋ ์ฌ์ ์์ ํค๊ฐ i ์ธ ๊ฒ์ ๊ฐ์ ๊ฐ์ ธ์จ๋ค
i๋ s์ ํญ๋ชฉ์ด๋ค

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
๋ iterable์ ํ๋์ ํํ๋ก ํฉ์น๊ธฐ
`*` ๋ฅผ ์ฌ์ฉํ๋ฉด ํํ๋ก ๋ถ๋ฆฌ ๊ฐ๋ฅ
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
๋ฐฐ์ด์ ๋๊ฐ์ด ๋ณต์ฌํ๋ ๋ฐ๋ ์์๋ก
```python
arr = [1, 2, 3, 4]
print(arr[::-1])
#[4, 3, 2, 1]
```


## 90๋ ํ์  with zip
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

## 90๋ ํ์ 
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
n๋ฒ์งธ๊น์ง

## [n:]
n๋ฒ์งธ๋ถํฐ

## map(function, iterable)
```python
a = '123'

# a์ ํญ๋ชฉ๋ค์ int() ์ ์ฉ convert to int
b = map(int, a)		#object
print(set(b))
# {1, 2, 3}
```

## sum(iterable, start)
List ํญ๋ชฉ๋ค์ ํฉ ๊ตฌํ๊ธฐ
```python
a = [1, 2, 3, 4, 0, 2]

print(sum(a))
#12
```

## map๊ณผ sum
๋ฌธ์์ด ๋ด ๋ฌธ์๋ค์ ํฉ ๊ตฌํ๊ธฐ
```python
a = '123402'

sum(map(int, a[:3]))
# 6
```
- `a[:3]` ์ '123'
- map์ผ๋ก '123'์ ํญ๋ชฉ๋ง๋ค int() ์ ์ฉ
- sum์ผ๋ก ๋ชจ๋ int๊ฐ ๋ '123'์ ๊ฐ ํญ๋ชฉ๋ค ๋ํจ
- `1 + 2 + 3 = 6`


## ์ผํญ์ฐ์ฐ์
```python
A if ์กฐ๊ฑด else B
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

- ๊ฐ์ ์ง์ ํ  ๋๋
```python
A = a if ์กฐ๊ฑด else b
```

```python
num = 5
type = 'odd' if num % 2 == 1 else 'even'
# odd
```

## str to list ๋ณํ
```python
text = 'hello'
a = list(text)
print(a)
# ['h', 'e', 'l', 'l', 'o']
```