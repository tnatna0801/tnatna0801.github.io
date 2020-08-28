---
layout: article
title: 가장 큰 수(정렬)
tag: 코테연습(프로그래머스)
---

## 문제

### 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

**제한 사항**

* numbers의 길이는 1 이상 100,000 이하입니다.
* numbers의 원소는 0 이상 1,000 이하입니다.
* 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

**입출력 예**

| numbers | return |
|---|---|
| [6, 10, 2] | 6210 |
| [3, 30, 34, 5, 9] | 9534330 |

### 문제 풀이
오늘도 결론은 구글링이었다. 그래도 스택/큐는 내가 비슷하게 풀 수 있어서 재밌었는데 이문제는 아이디어도 막혀서 재미없었다.

여러 블로그를 읽어 본 결과 핵심은 정렬 기준을 새로 만드는 것이었다. compare 함수를 override해서 숫자1+숫자2와 숫자2+숫자1 조합을 비교하는 것이다.

override하는 것은 생각도 못해서 자괴감 들었다.

내가 생각한 거라고는 모든 조합을 우선순위 큐에 삽입해서 내림차순으로 정렬한 뒤 제일 상위에 있는 가장 큰 수를 가져오는 것인데 생각해보니까 9와 34가 있을 경우 9가 우선순위가 더 높아야하는데 이를 어떻게 비교하지 싶어서 고민이었다. 9를 똑같이 2자리 수로 만들까 하다가 또 다른 예외가 있어서 포기한 아이디어다.

참고한 코드는 아래와 같다.
```java
import java.util.*;
 
class Solution {
    public String solution(int[] numbers) {
	String answer = new String();
    
	// int 배열 -> string 배열으로 변환
	String str_num[] = new String[numbers.length];
	for(int i=0; i<str_numbers.length; i++) {
		str_num[i] = String.valueOf(numbers[i]);
	}
	
	// string 배열을 내림차순의 정렬
    // Comparator 인터페이스를 사용하기 위해 메소드를 override(재정의)한다.
	Arrays.sort(str_num, new Comparator<String>() {
		@Override
		public int compare(String o1, String o2) {
			return (o2+o1).compareTo(o1+o2); //내림차순
		}
	});
	
	// 예외 처리 : 주어진 숫자 배열에서 같은 수가 중복 될 수도 있다.
    // ex)[0,0,0,0] 결과: 0000 => 0으로 나와야한다.
	if(str_num[0].startsWith("0")) { 
		answer += "0";
	} else {
		for(int j=0; j<str_num.length; j++) {
			answer += str_num[j];
		}
	}
	
	return answer;
}
}
```
[코드 출처](https://m.blog.naver.com/PostView.nhn?blogId=yongyos&logNo=221580402785&categoryNo=39&proxyReferer=https:%2F%2Fwww.google.com%2F)

## 회고
자바 풀이는 아니지만 도움이 되었던 [블로그](https://dailyheumsi.tistory.com/102)