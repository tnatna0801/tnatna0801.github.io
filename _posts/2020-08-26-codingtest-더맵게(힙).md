---
layout: article
title: 더 맵게(힙)
tag: 코테연습(프로그래머스)
---

## heap

* 우선 순위 큐를 위해 만들어진 자료 구조이다. ==> (그래서 java에서 heap을 만들 때 우선 순위 큐를 사용했구나)
* 완전 이진 트리의 일종
* 여러 개의 갑들 중에서 최댓 값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조 ==> 이 문제에서 heap을 사용하는 이유이다.
* 힙의 종류
	* 최대 힙
	* 최소 힙 ==> 문제에서 사용함
* heap의 표준적인 자료구조는 배열이다.



참고한 블로그

* [heap이란](https://gmlwjd9405.github.io/2018/05/10/data-structure-* heap.html)
* [우선순위 큐](https://siyoon210.tistory.com/117)


## 문제

### 문제 설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.

Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

**제한 사항**

* scoville의 길이는 2 이상 1,000,000 이하입니다.
* K는 0 이상 1,000,000,000 이하입니다.
* scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
* 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

**입출력 예**

| scoville | K | return |
|---|---|---|
| [1, 2, 3, 9, 10, 12] | 7 | 2 |

### 문제 풀이

다른 사람 풀이를 보고 이해해보았다.

최소 heap을 우선 순위 큐 클래스를 사용해 만들었다. 헷갈렸던 게 제한 사항 마지막 부분인 모든 음식의 스코빌 지수를 k이상으로 만들 수 없을 경우 -1을 return 한다는 부분 이었다.

우선 순위 큐가 어떻게 생겼는지 몰라서 헷갈렸던 것같다.

참고한 코드는 아래와 같다. 오늘도 [이 분](https://codevang.tistory.com/314)의 블로그로 공부했다.

```java
import java.util.PriorityQueue;

class Solution {

	public int solution(int[] scoville, int K) {

		int answer = 0;
		PriorityQueue<Integer> food = new PriorityQueue<>();

		for (int i = 0; i < scoville.length; i++) {
			food.add(scoville[i]);
		} 

		while (food.peek() < K) {

			if (food.size() < 2) {
				return -1;
			}

			int min = food.poll();
			int second = food.poll();
			food.add(min + 2 * second);

			answer++;
		}

		return answer;
	}
}
```

**설명**

* for문으로 주어진 음식들의 스코빌 지수를 food 우선 순위 큐에 삽입한다.
* while문에서 주어진 K보다 작은 스코빌 지수가 있으면 조건에 맞게 연산 후 정렬한다.
* if문은 모든 스코빌 지수가 K 이상으로 만들어 질 수 없을 경우 처리 문이다.
* 우선 순위 큐에서는 가장 작은 숫자를 우선 순위가 높다고 default로 생각하므로 poll() 함수로 가장 작은 스코빌 지수와 그다음으로 작은 스코빌 지수를 각각 변수에 담고 섞은 스코빌 지수를 add한다.
* answer로 몇번 섞었는지 저장한다.


## 회고

자료구조를 다시 공부해야 겠다는 생각밖에 안드는 문제였다.