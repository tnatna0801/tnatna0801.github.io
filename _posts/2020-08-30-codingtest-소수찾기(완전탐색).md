---
layout: article
title: 소수찾기(완전탐색)
tag: 코테연습(프로그래머스)
---

## 완전 탐색

가능한 방법을 전부 만들어 보는 알고리즘으로 컴퓨터의 빠른 계산 속도를 이용하는 방법이다.

1. Brute Force : for문과 if문을 이용하여 처음부터 끝까지 탐색하는 방법
2. 비트 마스크
3. 순열, 조합
4. 백트래킹
5. BFS

## 문제

### 문제 설명

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

** 제한사항 **

* numbers는 길이 1 이상 7 이하인 문자열입니다.
* numbers는 0~9까지 숫자만으로 이루어져 있습니다.
* 013은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

** 입출력 예 **

| numbers | return |
|---|---|
| 17 | 3 |
| 011 | 2 |

** 입출력 예 설명 **

* 예제 #1
	* [1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

* 예제 #2
	* [0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.
	* 11과 011은 같은 숫자로 취급합니다.


### 문제 풀이
주어진 숫자로 만들 수 있는 모든 순열을 구하는게 어려웠다. 소수 찾는 건 금방 생각해 냈는데 이부분을 시간 내에 해결하지 못해서 결국 구글링해보았다...

여러 사람의 블로그를 보았는데 다들 코드가 엄청 길다.

```java
    static boolean[] visited;
    static int ans;
    static char[] chs;
    static char[] select;   // 순열
    static HashSet<Integer> set;    // 소수 저장 (중복 제거)
    public static int solution(String numbers){
        chs = numbers.toCharArray();

        int size = numbers.length();
        visited = new boolean[size];

        ans = 0;
        set = new HashSet<>();
        
        for(int i=1; i<=size; i++){
            select = new char[i];
            perm(0, i, size);
        }

        return set.size();
    }

    public static void perm(int start, int r, int n){
        if(start == r){
            // 숫자로 변환
            String str = "";
            for(int i=0; i<select.length; i++)
                str += select[i];
            int num = Integer.parseInt(str);

            // 예외 처리
            if(num == 1 || num == 0)
                return;

            // 소수 인지 검사
            boolean flag = false;
            for(int i=2; i<=Math.sqrt(num); i++){
                if(num % i == 0)
                    flag = true;
            }

            if(!flag)
                set.add(num);
            return;
        }

        for(int i=0; i<n; i++){
            if(!visited[i]){
                visited[i] = true;
                select[start] = chs[i];
                perm(start+1, r, n);
                visited[i] = false;
            }
        }
    }
```

1. 모든 순열을 확인
	* start를 0부터 증가시키면서 select 배열에 순차적으로 삽입한다.
	* 재귀적으로 perm(start+1, r, n)을 호출한다.
2. 구한 문자를 숫자로 바꾸어 소수인지 판단 ==> 해당 숫자의 루트값까지 나누어 확인


### 참고
[코드 출처](https://hoho325.tistory.com/201) (정리 굿)
[코드 참고](https://codevang.tistory.com/299?category=827588) (정말 똑똑하신 분 같음)
[소수찾기](https://jar100.tistory.com/16)
[순열](https://gorakgarak.tistory.com/522)

## 회고
숫자 조합을 어떻게 배열에 저장할지 생각해보느라 오래걸렸다. 예전부터 순열 조합이 어려웠는데 극복이 안된다. 그래도 이번 문제로 다시 정리할 수 있어서 좋았다. 다시봐야지