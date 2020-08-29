---
layout: article
title: 프린터(스택)
tag: 코테연습(프로그래머스)
---

## 문제

### 문제 설명

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

** 제한사항 **

* 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
* 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
* location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

** 입출력 예 **

| priorities | location | return |
|---|---|---|
| [2, 1, 3, 2] | 2 | 1 |
| [1, 1, 9, 1, 1, 1] | 0 | 5 |

## 풀이

### 1. 첫 풀이(실패)

```java
import java.util.Queue;
import java.util.LinkedList;

class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 0;
        Queue<Integer> waiting = new LinkedList<Integer>();
        
        for(int p : priorities)
        {
            waiting.add(p);
        }
        
        int index = 1;
        int cur = waiting.poll();
        boolean flag = false;
        
        while(index < priorities.length)
        {
            for(int i = index; i<priorities.length; i++)
            {
                if(cur < priorities[i])
                {
                    flag = true;
                    waiting.add(cur);
                    cur = waiting.poll();
                    break;
                }
                else if (cur > priorities[i])
                {
                    flag = false;
                }
            }
            if(!flag)
                break;
            index++;
        }
        int tmp = priorities.length - (index-1);
        if(location >= tmp){
            answer = location - tmp + 1;
        }
        else
            answer = location + tmp + 1;
        return answer;
    }
}
```

채점하기 전 test는 통과했지만 채점하니까 모든 test에 실패 떴다. 슬프다.

**설명**

* 일단 대기 목록을 큐로 만들었다. (for문)
* while문에서 인쇄 디기 목록의 가장 앞에 있는 문서와 대기목록의 문서들의 중요도를 비교하고 가장 앞에 있는 문서가 중요도가 제일 크면 break (flag 이용) 아니면 반복문으로 대기목록을 문제 요구사항에 맞게 정렬한다.
* while문을 종료하고 내가 인쇄 요청한 문서가 몇 번째로 인쇄되는지 체크한다.

### 2. 다른 사람 풀이

```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
       int answer = 0;

        Queue<Integer> que = new LinkedList<>();

        for(int p : priorities){
            que.add(p);
        }

        Arrays.sort(priorities); 
        int len = priorities.length-1; 

        while(!que.isEmpty()){
            int cur = que.poll();

            if(cur == priorities[len - answer]){
                answer++;
                location--;
                if(location<0){
                    break;
                }
            }else{
                que.add(cur);
                location--;
                if(location<0){
                    location = que.size()-1;
                }
            }
        }
        return answer;
    }
}
```

** 설명 **

* 우선순위를 비교하기 위해서 오름 차순 정렬을 한게 가장 큰 차이점인 것 같다.
* 제일 큰 요소의 index를 len 변수에 저장해 두고 while문에서 비교한다.


코드 출처 : [참고한 블로그](https://n1tjrgns.tistory.com/190)

[추가적으로 배울 내용이 많은 블로그](https://codevang.tistory.com/310)


## 회고
문제가 너무 어렵다... 로직을 생각하는게 쉽지 않아..