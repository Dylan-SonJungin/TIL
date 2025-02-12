# Bit Masking

#### 정수의 이진수 표현을 활용하여 집합을 표현하는 기법

> 💡**비트마스킹의 장점**
>
> 1. 메모리 효율성: N개의 원소를 표현하는데 단 하나의 정수만 필요
> 2. 연산 속도: 비트 연산은 매우 빠름
> 3. 코드 간결성: 집합 연산을 비트 연산으로 간단히 표현 가능
>
> 
>
> 💡**주로 사용되는 경우**
>
> 1. 부분집합 생성
> 2. 상태 표현 (DP 문제에서 자주 사용)
> 3. 방문 체크
> 4. 플래그/옵션 관리
>
> 
>
> **💡시간복잡도**
>
> - 비트 연산: O(1)
> - N개 원소의 모든 부분집합 생성: O(2^N)





### 1. 기본 비트 마스크 연산자

```cpp
#include <iostream>
using namespace std;

// 기본적인 비트 연산자
// & (AND), | (OR), ^ (XOR), ~ (NOT), << (left shift), >> (right shift)
// shift = a의 모든 비트를 왼쪽 / 오른쪽으로 b칸 만큼 이동

// 1      = 00000001  (1)
// 1 << 1 = 00000010  (2)
// 1 << 2 = 00000100  (4)
// 1 << 3 = 00001000  (8)
// 1 << 4 = 00010000  (16)

void bitOperations() {
    // 비트 집합 표현
    int state = 0;              // 공집합 
    cout << bitset<8>(state) << endl; //[0000 0000]
    
    // 원소 추가 (i번째 비트를 1로)
    int i = 3;
    state |= (1 << i);         // 3번째 비트 켜기
    cout << bitset<8>(state) << endl; // [0000 1000]
    
    // 원소 제거
    state &= ~(1 << i);        // 3번째 비트 끄기 
    cout << bitset<8>(state) << endl; //[0000 0000]
    
    // 원소 존재 여부 확인
    if (state & (1 << i))
        cout << i << endl;
    else
        cout << i << endl; // [3]
        
    // 원소 토글 (0->1, 1->0)
    state ^= (1 << i);
    cout << bitset<8>(state) << endl; //[0000 1000]
    
    // 가장 낮은 켜져있는 비트 찾기
    int lowest_bit = (state & -state);
    cout << bitset<8>(lowest_bit) << endl; //[0000 1000]
    
    // 전체 집합
    int n = 4;
    int full = (1 << n) - 1;   // n개의 비트가 모두 1인 수
    cout << bitset<8>(full) << endl; // [0000 1111]
}
```



### 2. 자주 사용되는 비트 연산자

```cpp
// 1. 2의 거듭제곱 체크
bool isPowerOfTwo(int n) {
    return n && !(n & (n-1));
}

// 2. 켜져있는 비트 수 세기
int countBits(int n) {
    int count = 0;
    while(n) {
        count += n & 1;
        n >>= 1;
    }
    return count;
    // 또는 내장 함수 사용: __builtin_popcount(n)
}

// 3. 최하위 비트 끄기
n &= (n-1);

// 4. 최하위 비트 값 구하기
int lowest_bit = n & -n;
```



### 3. 시각화

### ADD 연산 (OR 사용)
```
예시: 3을 추가하고 싶을 때 (1 << 2)

현재 상태:    0000
1 << 2:       0100  (3번째 비트를 1로)
OR 연산(|):   0100  (결과)
```

OR(|)를 사용하는 이유:
- 다른 비트는 그대로 유지하면서
- 원하는 위치만 1로 설정할 수 있기 때문
- **이미 1이어도 1이 유지됨 (중복 추가 방지)**

### REMOVE 연산 (&~)
```
예시: 3을 제거하고 싶을 때 (~(1 << 2))

1 << 2:       0100
~ 연산:       1011  (비트 반전)
현재 상태:    0100
AND 연산(&):  0000  (결과)
```

&~ 를 사용하는 이유:
- ~ 로 해당 비트만 0이고 나머지는 1인 패턴을 만들고
- AND(&)로 해당 위치만 0으로 만들고 나머지는 유지
- **이미 0이어도 0이 유지됨 (중복 제거 방지)**

각각의 연산을 단계별로 시각화해서 설명해드리겠습니다:

###  원소 존재 여부 확인 (state & (1 << i))
```
예시: i=3일 때

현재 state:   0000 1000
1 << 3:       0000 1000
AND 연산(&):  0000 1000  (0이 아니면 존재, 0이면 없음)
```
- AND 연산으로 해당 위치의 비트만 확인
- 결과가 0이 아니면 해당 위치의 비트가 1
- 결과가 0이면 해당 위치의 비트가 0

### 토글 연산 (state ^= (1 << i))
```
예시: i=3일 때

현재 state:   0000 1000
1 << 3:       0000 1000
XOR 연산(^):  0000 0000  (같으면 0, 다르면 1)
```
- XOR의 특성: 같은 비트면 0, 다른 비트면 1
- 1이면 0으로, 0이면 1로 바꿔줌

### 가장 낮은 켜진 비트 찾기 (state & -state)
```
state:        0000 1000
-state:       1111 1000 (2의 보수: 비트 반전 후 +1)
AND 연산(&):  0000 1000
```
- 음수는 2의 보수로 표현 (모든 비트 반전 후 +1)
- AND 연산하면 최하위 1비트만 남음

### 전체 집합 ((1 << n) - 1)
```
n=4일 때:
1 << 4:       0001 0000
-1:           0000 1111  (결과)
```
- 1을 n만큼 왼쪽으로 시프트하면 n+1번째 비트만 1
- 여기서 1을 빼면 n개의 비트가 모두 1이 됨