---
layout: article
title: 기능개발(스택/큐)
tag: 코테연습(프로그래머스)
---

## 문제

### 문제 설명

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

**제한 사항**

* 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
* 작업 진도는 100 미만의 자연수입니다.
* 작업 속도는 100 이하의 자연수입니다.
* 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

**입출력 예**

| progresses | speeds | return |
|---|---|---|---|
| [93,30,55] | [1,30,5] | [2,1] |

**입출력 예 설명**

첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.

두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다.

하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.

세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

### 풀이

##### 첫번째 풀이(실패 ㅜ)

```java
import java.util.Queue;
import java.util.LinkedList;
import java.util.ArrayList;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        Queue<Integer> dq = new LinkedList<Integer>();
        
        int day = 0;
        for(int i = 0; i<progresses.length; i++){
            day = (100-progresses[i]) / speeds[i];
            if((100-progresses[i]) % speeds[i] != 0 )
                day += 1;
            dq.offer(day);
        }
        
        ArrayList<Integer> tmpanswer = new ArrayList<>();
        
        while(dq.isEmpty() == false){
            int count = 1;
            int tmp = dq.peek();
            dq.poll();
            if(dq.isEmpty() != true){
                if(tmp >= dq.peek()){
                    count++;
                    dq.poll();
                }
            }
            tmpanswer.add(count);
        }
        int index = 0;
        int[] answer = new int[tmpanswer.size()];
        for(int value:tmpanswer)
        {
            answer[index++] = value;
        }
        return answer;
    }
}
```

![image](https://user-images.githubusercontent.com/48270067/90888497-56235100-e3f1-11ea-8671-da7dc30f8aa8.png)

**설명**

* 먼저 첫번째 for문으로 각 작업마다 몇일이 걸리는지 계산하여 queue에 삽입하였다.
* 두번째로 answer의 크기를 아직 모르기 때문에 ArrayList로 임시로 배포되는 개수를 저장하였다. 
	* while문을 사용해 queue의 작업 일수를 비교하면서 배포마다 몇 개의 작업이 배포되는지 count번수에 저장하고 이를 ArrayList에 add하였다.
	* 변수 tmp에 queue의 첫번째 요소를 저장하고 해당 요소를 queue에서 제거한다.
	* tmp의 값과 현재 queue의 첫번째 요소(peek())을 비교해 tmp가 크거나 같다면 첫번째 기능을 배포할때 두번째 기능도 같이 배포되므로 count를 증가하고 poll()로 queue에서 제거하였다.
	* 만약 두번째 요소(현재 queue의 top)가 더 오래 걸린다면 tmp의 기능만 배포되므로 1이 배포된다.
	* 마지막 for문에서는 ArrayList의 값을 answer 배열에 넣었다.
* 왜 test를 두개만 통과했는지 모르겠다. 어떤 예외를 처리해주지 않은 걸까..

##### 두번째 풀이

코드의 순서가 문제였다. 정말 입출력 예제만 통과할만한 코드였는데 머릿속으로 잘 생각을 못하겠으면 제발 손으로 써보자 나자신..

```java
import java.util.Queue;
import java.util.LinkedList;
import java.util.ArrayList;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        Queue<Integer> dq = new LinkedList<Integer>();
        
        int day = 0;
        for(int i = 0; i<progresses.length; i++){
            day = (100-progresses[i]) / speeds[i];
            if((100-progresses[i]) % speeds[i] != 0 )
                day += 1;
            dq.offer(day);
        }
        
        ArrayList<Integer> tmpanswer = new ArrayList<>();
        
        int prevTop = dq.poll();
        int count = 1;
        while(!dq.isEmpty()){
            int curTop = dq.poll();
            if(prevTop >= curTop){
                count++;
            }
            else
            {
                tmpanswer.add(count);
                count = 1;
                prevTop = curTop;     
            } 
        }
        tmpanswer.add(count);
        int index = 0;
        int[] answer = new int[tmpanswer.size()];
        for(int value:tmpanswer)
        {
            answer[index++] = value;
        }
        return answer;
    }
}
```

**주요 로직 설명**

* 일단 변수 명을 좀더 명확하게 바꾸었다. prevTop은 queue의 첫번째 요소고 curTop은 첫번째 요소를 꺼낸 뒤(poll()) 현재 queue의 첫번째 요소를 말한다.
* 필요없는 if문 조건을 제거하였다.
* 앞선 기능보다 다음 기능을 개발하는데 오래 걸리면 (else문) 앞선 기능을 배표할 때 해당 기능만 배포하기 때문에 count는 1이다.
* queue의 모든 요소를 반복해야하기 때문에 prevTop = curTop을 해주어 처리해준다.


## 회고
한번에 푸는 법이 없다.... 내일부터는 푸는데 걸리는 시간을 재서 풀이와 함께 기록해볼려고 한다.