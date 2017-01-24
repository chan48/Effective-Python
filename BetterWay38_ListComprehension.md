# Better Way 8. 리스트 컴프리헨션에서 표현식을 두 개 넘게 쓰지 말자.

#### 36쪽.
#### 2017/01/24 작성.


## Part 1.

리스트 컴프리헨션은 [기본 사용법뿐 아니라](https://github.com/shoark7/Effective-Python/blob/master/BetterWay07_useListComp.py) 다중 루프도 지원한다.  
예를 들어 행렬을 평평한 리스트 하나로 간략화한다고 가정하자.

```python
complex_list = [[1 ,2 ,3], [4 ,5 ,6], [7 ,8 ,9]]
flat_one = [x for row in complex_list for x in row]
print(flat_one)

>>> [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

*flat_one*의 해석 순서는 **왼쪽부터 오른쪽**으로 읽혀들어간다.  
이 예제에서는 간단하고 읽기 쉽고 합당한 다중 루프를 사용했다.  
다중 루프의 또 다른 합당과 사용법은 입력 리스트의 레이아웃을 두 레벨로 중복해서 구성하는 것이다.  

예를 들어 2차원 행렬의 각 셀에 있는 값의 제곱을 구한다고 하자.  
이 표현식은 추가로 [] 문자를 사용하기 때문에 그리 좋아 보이진 않지만 그래도 이해하기는 쉽다.

```python
squared_list = [[x ** 2 for x in row] for row in complex_list]
print(squared_list)

>>> [[1, 4, 9], [16, 25, 36], [49, 64, 81]]
```

혼동하지 말자. 여기서는 **오른쪽에서부터 왼쪽**으로 가야한다.  
~사실 리스트 컴프리헨션은 작동방식이 이상하다고 불평을 간간히 듣는다.~  

이 표현식을 다른 루프에 넣는다면 리스트 컴프리헨션이 여러 줄로 구분해야 할 정도로 길어진다.

```python
my_lists = [
  [[1,2,3], [4,5,6]], 
  ...
  ...
]

flat = [x for sublist1 in my_lists
	for sublist2 in sublist1
	for x in sublist2]

```
정말 고된 식이다. 이 정도되면 리스트 컴프리헨션이 다른 방법보다 그다지 짧아보이지 않는다.  
이번엔 일반 루프문으로 같은 결과를 만들어본다.  
이 버전은 들여쓰기를 사용해서 리스트 컴프리헨션보다 이해하기 쉽다.  
즉, 리스트 컴프리헨션이 언제나 최선을 아니라는 의미.

flat = []
for sublist1 in my_list2:
    for sublist2 in sublist1:
        flat.extend(sublist2)
```
<br>
<BR>

## Part 2.

리스트 컴프리헨션은 다중 if 조건도 지원한다.  
같은 루프 레벨에 여러 조건이 있으면 암시적인 _and_ 표현이 된다.  

예를 들어 숫자로 구성된 리스트에서 `4보다 큰` _and_ `짝수` 값만 가지고 온다면 다음 두 리스트 컴프리헨션은 동일하다.  

```python
a = [1 ,2 ,3 ,4 ,5 , 6, 7, 8, 9,10]
b = [x for x in a if x > 4 if x % 2 == 0]
c = [x for x in a if x > 4 and  x % 2 == 0]

# b와 c는 동일!
```

조건은 루프의 각 레벨에서 **for 표현식 뒤에 설정할 수 있다.**  
예를 들어 주어진 행렬에서 각 행(row)의 합이 10이상일 때 그 행의 셀 중으로 나누어 떨어지는 셀을 구해보자.  
다음처럼 리스트 컴프리헨션으로 표현하면 간단하지만 이해하기는 매우 어렵다.

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
filtered = [[x for x in row if x % 3 == 0] for row in matrix if sum(row) >= 10]

print(filtered)

>>> [[6], [9]]
```
위 식은  

1. _matrix_에서 행의 합이 10이 넘는건 `[4, 5, 6]`, `[7, 8, 9]`이며,
2. 이 행들 중에서 3의 배수인 셀은 6, 9가 나온다.

난해한 예로 이런 표현식은 가급적 피하는 것이 좋다.  
이런 코드는 다른 사람들이 이해하기 매우 어렵다. **몇 줄을 절약한 장점이 나중에 겪을 어려움보다 크지 않다.**  

경험칙으로 볼 때, 리스트 컴프리헨션을 사용할 때는 표현식이 두 개를 넘어가면 피해야 한다.  
조건 두 개, 루프 두 개, 혹은 조건 한 개와 루프 한 개 정도면 된다.  
이것보다 복잡해지면 그냥 if문과 for문을 사용하고 헬퍼 함수를 작성해야 한다.




### 핵심정리

1. 리스트 컴프리헨션은 다중 루프와 루프 레벨별 다중 조건을 지원한다.
2. 표현식이 두 개가 넘게 들어 있는 리스트 컴프리헨션은 이해하기 매우 어려우므로 피해야 한다.