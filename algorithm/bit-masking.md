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

void bitOperations() {
    // 비트 집합 표현
    int state = 0;              // 공집합
    
    // 원소 추가 (i번째 비트를 1로)
    int i = 3;
    state |= (1 << i);         // 3번째 비트 켜기
    
    // 원소 제거
    state &= ~(1 << i);        // 3번째 비트 끄기
    
    // 원소 존재 여부 확인
    if (state & (1 << i))
        cout << i << "번째 비트가 켜져있음" << endl;
        
    // 원소 토글 (0->1, 1->0)
    state ^= (1 << i);
    
    // 가장 낮은 켜져있는 비트 찾기
    int lowest_bit = (state & -state);
    
    // 전체 집합
    int n = 10;
    int full = (1 << n) - 1;   // n개의 비트가 모두 1인 수
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