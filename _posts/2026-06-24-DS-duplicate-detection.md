---
title: "[자료구조] 알고리즘 수행 시간 비교: Duplicate Detection"
date: 2026-06-22 23:30:00 +0900
categories: [전공공부, 자료구조]  # [대분류, 소분류] 구조!
tags: [python, BigO, 자료구조]    # 소문자 해시태그들
math: true
---

안녕하세요! 이번에는 자료구조 과목에서 가장 처음 배웠던 시간복잡도에 대해서 알려드리려고 합니다. 그리고 아래 제가 과제를 하며 했던 시간복잡도 비교 실험을 보여드리고자 합니다.

**시간복잡도**란 입력 n에 대하여 그 자료구조 혹은 알고리즘에 대한 전체 실행 시간을 말합니다. 하지만 전체 실행 시간을 엄밀하게 따지기란 너무 복잡합니다. 컴퓨터의 성능과 각 연산에 대한 수행 시간을 정확히 알아내서 총 시간을 계산하기는 어렵죠. 그렇고 사실 그렇게까지 엄밀한 단위로 계산할 필요성도 적습니다. 

따라서 등장한 것이 **Big-O** 표기법입니다. 코드를 분석해서 가장 영향을 크게 미치는 n에 대해 표시하는 것입니다. $O(n)$, $O(n^2)$, $O(\sqrt n)$, $O(1)$ 등으로 표기할 수 있습니다. 

><small><i><b>더 알아보기: 알고리즘 수행 시간 계산</b>  
>big-O로 셀 때 계산법을 더 자세히 알고 싶으신 분은 **가상컴퓨터(Virtual Machine)** 와 **기본 연산(primitive operation)** 을 찾아보시길 바랍니다! 여기서 다 정리하기에 너무 길지만, 해당 내용을 찾아보면 더 깊이있게 이해하는데 도움이 됩니다! </i></small>

아래 내용은 Duplicate Detection 코드를 세 가지 시간복잡도에 맞게 짠 다음 그 수행 시간을 비교하는 실험입니다. 이론적으로 배운 big-O처럼 실제로 수행 시간이 그렇게 나올지 검증합니다. 먼저 각 코드를 살펴보겠습니다.

## 1. Duplicate Detection Code

먼저 duplicate detection이란 말 그대로 중복된 원소가 있는지 검사하는 것입니다. 코드에서 list A를 생성한 뒤에, A 안에 중복된 원소가 있는지 세 가지 방법으로 검사합니다.

``` python
import random
import time
import matplotlib.pyplot as plt

## [0] list A 생성

n = int(input("정수 n을 입력하시오. (설정 범위 [-n, n]) : "))    # n 입력받기
random.seed(12)
A =[]                                                     # list A 생성
for _ in range (0, n):                                    # [-n, n] 범위의 수 랜덤하게 생성하여 리스트 A에 저장
    a = random.randrange(-n, n)
    A.append(a)
# print(f"list A: {A}")                                   # A 출력

```

### 1-1 이중 for문

``` python
## [1] O(n^2) 알고리즘
def ON2_count(A):
    before = time.process_time()
    cnt = 0
    c=0
    for i in range(0, n):                                 # 이중 for문으로 모든 경우의 수 비교
        duplicate = False                                 # duplicate를 False로 가정하기
        for j in range(0, i):                             # A[i] == A[j]인 경우, duplicate를 True로 바꾸고 j반복문 빠져나오기
            if (A[i] == A[j]):
                duplicate = True
                break
        if duplicate == False:                            # duplicate가 False일 경우, i에 해당하는 값 앞부분에서 유일하므로 카운트하기
            cnt += 1
    after = time.process_time()
# print
    print("== [1] O(n^2) algorithm ==")
    print(f"result: {cnt}")
    print(f"time: {after - before}")
```
가장 단순한 방법으로, 리스트를 1대1로 각각 모든 원소들과 한 번씩 돌아가며 비교하는 방법 입니다. 가장 시간이 오래 걸리지만 알고리즘이 단순하고 구현하기도 쉽습니다. 바깥 for문으로 돌아가며 하나씩 원소를 붙잡고 안쪽 for문을 돌려서 모든 리스트 원소와 하나씩 비교합니다. 따라서 반복문이 $n \times n$만큼 반복되기 때문에 $O(n^2)$입니다.

### 1-2 정렬 후 비교
``` python
## [2] O(nlogn) 알고리즘
def ONLOGN_count(A):
    before = time.process_time()
    A.sort()                                              # 정렬
    cnt = 0
    for i in range(0, n-1):                               # 인덱스에러 방지를 위해서 n-2까지 반복 (i+1)과 비교하기 때문
        if A[i] != A[i+1]:                                # A[i] 와 A[i+1]이 다르면 카운트하기
            cnt += 1
    cnt += 1
    after = time.process_time()
# print
    print("== [2] O(nlogn) algorithm ==")
    print(f"result: {cnt}")
    print(f"time: {after - before}")
```
리스트를 내장함수로 정렬한 다음 비교하는 방식입니다. 정렬한다면, 중복된 원소가 있을 경우 연속으로 나오게 됩니다. 따라서 각각 옆 원소와 비교하면 되기 때문에 오래 걸리지 않습니다. 다만 내장함수로 정렬하는데 $O(nlogn)$ 만큼 걸리게 됩니다.

### 1-3 dictionary 이용

``` python
## [3] O(n) 알고리즘 
def ON_count(A):
    before = time.process_time()
    cnt = 0
    dA = dict.fromkeys(A)                                 # dA라는 dictionary에 list A를 각각 key가 되도록 넣기
    cnt = len(dA)                                         # dA의 길이 측정(dictionary key는 중복을 허용하지 않음!)
    after = time.process_time()
    # print
    print("== [3] O(n) algorithm ==")
    print(f"result: {cnt}")
    print(f"time: {after - before}")

## [4] 실행

ON2_count(A)
ONLOGN_count(A)
ON_count(A)
print ("============================")
print(f"correct : {len(set(A))}")
```
dictionary 자료구조를 이용하는 방식은 이 자료구조의
특성을 이용합니다. Dictionary는 key와 value가 짝지어 저장되는 형태로,
여기서 key는 중복될 수 없습니다. 이 점을 활용하여 리스트에 저장된 수들을
fromkeys()라는 메서드를 통해 리스트의 값들을 key로 하나씩 저장하고, 이
과정은 n번 반복되는데 그 속에서 중복된 값들은 key로 저장되지 않고
제외됩니다. 따라서 남은 key의 수를 세면 이는 서로 다른 숫자의 개수가 됩니다.

### 1-4. 실행 시간 측정 알고리즘

앞서 코드를 보시면 아시겠지만, 수행 시간 측정을 위해서 `time` 라이브러리를 사용합니다. 변수 `before`과 `after`을 tune.process_time()으로 측정하고, `after-before`로 실제 수행 시간을 구합니다.

## 2. 실행 시간 측정 결과 및 비교 그래프

### [Table 1] 입력 크기 n에 따른 각 알고리즘의 수행 시간 (단위:초)
<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 28%" />
<col style="width: 30%" />
<col style="width: 31%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;">n</th>
<th style="text-align: center;">O(n²) (초)</th>
<th style="text-align: center;">O(n log n) (초)</th>
<th style="text-align: center;">O(n) (초)</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: center;">100</td>
<td style="text-align: center;">0.0006399999999999982</td>
<td style="text-align: center;">3.699999999999884e-05</td>
<td style="text-align: center;">6.499999999999909e-05</td>
</tr>
<tr>
<td style="text-align: center;">1,000</td>
<td style="text-align: center;">0.028967999999999997</td>
<td style="text-align: center;">0.0002530000000000032</td>
<td style="text-align: center;">8.99999999999998e-05</td>
</tr>
<tr>
<td style="text-align: center;">5,000</td>
<td style="text-align: center;">0.296429</td>
<td style="text-align: center;">0.0006120000000000014</td>
<td style="text-align: center;">0.00016300000000002424</td>
</tr>
<tr>
<td style="text-align: center;">10,000</td>
<td style="text-align: center;">1.075778</td>
<td style="text-align: center;">0.0012690000000001866</td>
<td style="text-align: center;">0.000304000000000082</td>
</tr>
<tr>
<td style="text-align: center;">50,000</td>
<td style="text-align: center;">26.467117000000002</td>
<td style="text-align: center;">0.007134999999998115</td>
<td style="text-align: center;">0.0014759999999967022</td>
</tr>
<tr>
<td style="text-align: center;">100,000</td>
<td style="text-align: center;">105.120362</td>
<td style="text-align: center;">0.01566200000000606</td>
<td style="text-align: center;">0.0029609999999991032</td>
</tr>
</tbody>
</table>

>My computer environment: M1 chip (8-core CPU), macOS, Python 3.14.3, VS
Code

### [Figure 1] compare each algorithm
<figure>
<img src="/assets/img/DSHW1_Figure_1.png"
alt="comparison each algorithm"
style="width: 100%; aspect-ratio: 16 / 9" />
</figure>

### [Figure 2] compare each algorithm (y-log scale)
<figure>
<img src="/assets/img/DSHW1_Figure_2.png"
alt="comparison each algorithm with y-log scale"
style="width: 100%; aspect-ratio: 16 / 9" />
</figure>

---

## 3. 분석 및 고찰

### 3-1 각 알고리즘의 시간복잡도 설명

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 19%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;">알고리즘</th>
<th style="text-align: center;">시간복잡도</th>
<th style="text-align: center;">핵심 아이디어 설명</th>
</tr>
</thead>
<tbody>
<tr>
<td>일일이 비교 방식</td>
<td style="text-align: center;">O(n²)</td>
<td>이중 for문을 이용하여 중복되지 않을 경우 세기</td>
</tr>
<tr>
<td>정렬 후 비교 방식</td>
<td style="text-align: center;">O(n log n)</td>
<td>정렬 후 다음 수와 다를 경우 세기</td>
</tr>
<tr>
<td>dict 활용 방식</td>
<td style="text-align: center;">O(n) 평균</td>
<td>Dictionary 자료구조 key 활용</td>
</tr>
</tbody>
</table>

### 3-2 이론적 예측과 실제 측정 결과 비교

이론적으로 각 예측 값은 시간 **Big-O로 표기한 각 시간복잡도**에 해당합니다.
일일이 비교하는 방식의 경우에는 이중 for문을 이용하므로 **O(n²)** 만큼
걸리며, 정렬 후 비교하는 방식은 비교하는 것은 for문 한 번으로 n번이면
끝나지만 정렬하는데 sort()라는 O(n log n)만큼 시간이 걸리는 메서드를
사용하기 때문에 최종적으로 **O(n log n)** 만큼 걸립니다. 마지막으로 dictionary
활용 방식의 경우에는 이 자료구조의 key라는 것의 특성을 이용하는데, Key는
중복된 값을 저장하지 않으므로 이 과정은 한 번 key로 저장하는데 n번
반복하므로 **O(n)** 만큼 걸립니다.

이러한 예측 값들은 실제로 측정한 결과와 양상이 일치합니다. 시간복잡도의 각
Big-O 표기법의 식과 실제 측정한 결과를 그래프로 그린 것의 모양이
비슷하기 때문에 이를 통해 이론적 예측이 실제에도 적용된다고 볼 수 있습니다.

### 3-3 n이 커질 때 수행 시간 차이 변화

입력 크기 n이 증가함에 따라 세 알고리즘의 수행 시간 차이는 변합니다.
O(n²)의 경우 다른 알고리즘보다 확연하게 증가합니다. **Figure 1**을 보면,
빨간색 그래프만 눈에 띄게 커지는 모습이다. 반면에 O(n log n)과 O(n)은
비슷해 보입니다. 둘의 차이점을 더 잘 표현하기 위해서 y값(수행 시간)에
log를 취해 표현한 그래프가 **Figure 2**입니다. 이 그래프를 보면 O(n log n)과
O(n)과 확실히 차이가 난다는 것을 알 수 있습니다.

### 3-4 시간복잡도가 실제 성능에 미치는 영향 및 소감

사실 작은 n에 대해서는 비슷한 성능을 보였지만 n이 10,000
이상부터는 표를 보면 알 수 있겠지만 가장 느린 O(n²) 알고리즘이 1초를
넘기게 됩니다. 그리고 50, 000과 100,000에서는 확실히 기다려야 할 정도로
체감되었습니다. 

어떤 프로그램을 실행할 때, 잠깐 실행되는 그 시간이 1초
이상이라면 정말 길게 느껴질 것입니다. 이러한 수행 시간은 누적되어 사용자는
더 크게 속도를 체감하게 될 것입니다. 이번 탐구를 통해서 이를 더 크게
느꼈고, 항상 O(n) 혹은 O(1)에 해당하는 알고리즘을 만들도록 노력해야 하는
이유를 직접 체감하며 big-O 시간복잡도 표기가 유효함을 검증하였습니다!