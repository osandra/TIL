### 소수 판별
소수: 1보다 큰 자연수 중에서 자기 자신을 제외한 자연수로는 나누어떨어지지 않는 자연수 (1은 소수가 아님)

- **방법 1**: 2부터 X-1까지 나누어떨어지는 수가 있으면 소수가 아님
```python
def is_prime(x):
    for i in range(2,x):
        if x % i == 0:
            return False
    return True

print(is_prime(4)) # False
print(is_prime(7)) # True

# 위 방법은 O(X) 소요
# 소수 확인 시 가운데 약수까지만 확인해도 된다. 2*8 = 8*2
```

- **방법2**: 제곱근(X)까지만 검사하는 방식
```python
import math
def is_prime_number(x):
    for i in range(2,int(math.sqrt(x))+1): #제곱근 수까지만 체크.
        if x% i == 0:
            return False
    return True


print(is_prime_number(4)) # False
print(is_prime_number(7)) # True
# O(제곱근 X) 소요
```


- **방법3**: 에라토스테네스의 체 (NloglogN) 소요

    장점: 빠름, 단점: 메모리가 많이 필요. N이 1,000,000 이내로 주어질 때 적합함.
1. 2부터 n까지 모든 자연수 나열 
2. 젤 작은 수 (i) 찾기 
3. 남은 수 중 작은수(i) 배수 모두 제거(i는 제거 안 함) 
4. 반복할 수 없을 때까지 해당 과정(2~3) 반복. i는 n의 제곱근(가운데 약수)까지만 증가

```python
import math
n = 1000
array = [True for i in range(n+1)]
for i in range(2,int(math.sqrt(n))+1):
    if array[i] == True: 
        j = 2 #i를 제외한 i의 배수 모두 지우기
        while i*j <= n:
            array[i*j] = False
            j+=1

#모든 소수 출력
for i in range(2,n+1):
    if array[i]: #True만 출력
        print(i,end = ' ')
```

- [백준-소수 구하기 문제(1929)](../practice_algo/others/소수구하기.md)
- [백준-약수 구하기 문제(2501)](../practice_algo/others/약수구하기.md)

---
### 투 포인터
2개의 변수를 이용해 리스트 상의 2개 점의 위치를 기록하며 처리하는 알고리즘.

시작점을 오른쪽으로 이동시키면 항상 합이 감소하고 끝점을 오른쪽으로 이동시키면 항상 합이 증가하기 때문에 해결 가능

ex) 특정합 합을 가지는 부분 연속 수열 찾기

```python
n= 5 # 데이터 개수
m = 5 # 찾고자 하는 부분합 값
data = [1,2,3,2,5]
count = 0 #부분합 등장 수
interval_sum = 0
end = 0

# start를 차례대로 증가시키며 반복
for start in range(n):
    # end 를 가능한 만큼 이동시키기
    while interval_sum < m and end < n:
        interval_sum += data[end] #interval_sum이 부분합보다 작고 끝 점이 n보다 작으면 끝점 값을 부분합에 더하기
        end += 1
    # 만약 interval_sum > m 인데 end < n인 경우는 계속 진행해봤자 계속 interval_sum 값만 커지므로 확인할 필요 없음
    if interval_sum == m: #부분합과 일치하면 count 증가
        count += 1
    # 다음 진행할 for 문에선 앞의 점(위치)가 한 칸 뒤로 이동하니까 더해줬던 값 빼기
    interval_sum -= data[start] # 다음 진행 시 앞에 원소 하나 빠지니까.
```
---
### 정렬된 두 데이터를 합하기(정렬)
병합정렬 중 합치는 과정에서 사용함

```python
n,m = 3,4
a = [1,3,5]
b = [2,4,6,8]

# 리스트 a,b의 모든 원소를 담을 수 있는 크기의 결과 리스트 초기화
result = [0] * (n+m)
i=j=k=0
while i<n or j<m:
    if j>=m or (i<n and a[i]<=b[i]): #리스트 b의 모든 원소가 처리되었거나 리스트 a의 원소가 더 작을 때
         result[k] = a[i]
         i += 1
    else: #리스트 a의 모든 원소가 모두 처리되었거나 리스트 b의 원소가 더 작을 때
        result[k] = b[j]
        j+=1
    k+=1

for i in result:
    print(i, end = ' ')
```
---
### 구간 합 계산
리스트 맨 앞부터 특정 위치까지의 합을 미리 배열에 구해놓아서 구간 합을 계산하는 방법.
```python
# P[0]:0번째 수까지의 합
# P[1]:1번째 수까지의 합 (인덱스 0 원소까지 합)
# P[3]: 3번째 수까지의 합 (인덱스 2 원소까지 합)

#데이터 개수, 전체 데이터 선언
n = 5
data = [10,20,30,40,50]

# 접두사 합 계산
sum_value = 0
prefix_sum = [0]
for i in data:
    sum_value += i
    prefix_sum.append(sum_value)
# 접두사 합 준비 완료
# 구간 합 계산 3번째 수부터 네번쨰 수까지
left = 3
right = 4
print(prefix_sum[right]-prefix_sum[left-1])
```
---
### 순열과 조합
순열: 서로 다른 n개에서 r개를 선택하여 일렬로 나열하는 것.

조합: 서로 다른 n개에서 **순서에 상관없이** 서로 다른 r개를 선택하는 것.
```python
# 순열
import itertools
data=[1,2,3]
for x in itertools.permutations(data,2):
    print(list(x), end= ' ') #[1, 2] [1, 3] [2, 1] [2, 3] [3, 1] [3, 2]
print()
# 조합
data = [1,2,3]
for x in itertools.combinations(data,2):
    print(list(x),end= '') #[1, 2][1, 3][2, 3]
```

#### 출처: 이것이 취업을 위한 코딩 테스트다 with 파이썬