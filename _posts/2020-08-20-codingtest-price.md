---
layout: article
title: 주식 가격
tag: 코테연습(프로그래머스)
---



### 문제풀이

#### 배열(실패)

```java
class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        for(int i = 0; i<prices.length-1; i++){
            for(int j = i+1; j<prices.length; j++){
                if(prices[i] <= prices[j]){
                    answer[i] += 1;
                }
                else if(prices[i] > prices[j])
                {
                    answer[i] = 1;
                    break;
                }else if(i == prices.length-2)
                {    answer[i] = 0; }
            }
        }
        return answer;
    }
}
```

* for문과 배열을 사용해서 문제를 풀어보았다.
* 하지만 test 1개를 빼고 다 통과하지 못하였다. 왤까ㅜ


![실패](https://user-images.githubusercontent.com/48270067/90754239-98c32b80-e314-11ea-959b-27e53a649433.png)

#### 스택
결국 시간안에 풀지 못해서 다른 사람들의 풀이를 보고 공부했다. 다들 쉽게 풀어나간 것 같은데 아쉽다.

**처음 참고한 사람의 코드**
```java
import java.util.Arrays;
import java.util.Stack;

class Solution {

	public static void main(String[] args) {

		int[] prices = new int[] { 1, 2, 3, 2, 3 };
		Solution s = new Solution();
		System.out.println(Arrays.toString(s.solution(prices)));

	}

	public int[] solution(int[] prices) {

		int[] answer = new int[prices.length];
		Stack<int[]> stack = new Stack<>();

		// 뒤에서부터 순회
		for (int i = prices.length - 2; i >= 0; i--) {

			// 앞쪽의 숫자가 뒷쪽의 숫자보다 클 경우
			if (prices[i] > prices[i + 1]) {

				answer[i] = 1;
				stack.push(new int[] { prices[i + 1], i + 1 });

			// 앞쪽의 숫자가 뒷쪽의 숫자보다 작거나 같은 경우
			} else {

				// 스택에서 자신보다 낮은 값이 나올때까지 모두 지워줌
				while (stack.size() > 0 && stack.peek()[0] >= prices[i]) {
					stack.pop();
				}

				// 스택이 빌 경우 (뒷쪽의 모든 값이 자신보다 큼)
				if (stack.size() == 0) {
					answer[i] = prices.length - i - 1;

				// 스택에 자신보다 낮은 숫자가 남아있다면 해당 인덱스와 자신의 인덱스를 이용해 계산
				} else {
					answer[i] = stack.peek()[1] - i;
				}
			}
		}

		return answer;
	}
}
```
'처음 나오는 작은 숫자의 인덱스만 알면된다.' 이게 문제의 핵심이었다. 나는 반대로 비교 대상이 기준 보다 작을 때는 break로 반복문을 빠져나오게 했었다. (실패)

뒤에서부터 순회를 하면서 스택에 쌓고 자신보다 큰 숫자를 모두 지운다?? ==> 이해가 안감

논리를 이해하지 못했다. 다시 볼것

[참고한 블로그](https://codevang.tistory.com/313)

**두번째 참고한 코드**
```java

class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        int count = 0;
        
        for(int i=0; i<prices.length-1; i++) {
            count = 0;
            for(int j=i+1; j<prices.length; j++) {
                if(prices[i]<=prices[j]) {
                    count++;
                } else {
                    count++;
                    break;
                }
            }
            answer[i] = count;
        }
        
        answer[prices.length-1] = 0;
        
        
        return answer;
    }
}

```

[이 분](https://ju-nam2.tistory.com/35)은 스택을 이용하지 않고 배열로 푸셨는데 나랑 비슷한 것같지만 코드를 돌려보니 test를 다 통과했다.

분석하면서 내 코드가 어디가 잘 못 되었는지 살펴보았다.

* count 변수를 썼다. 몇초동안 주식 가격이 떨어지지 않았는지 count하는 변수이다.
* else if문을 사용하지 않고 for문 마지막에서 answer[마지막 인덱스] = 0을 처리해주었다. 이는 항상 만족하는 것으로 굳이 elseif로 하지 않아도 되는게 맞는 듯 하다.
* 기본적인 논리는 동일하다.


[참고한 블로그](https://ju-nam2.tistory.com/35)

## 회고
level 2에서.. 심지어 스택과 큐에서 이렇게 막힌다는 것이 충격이다..
