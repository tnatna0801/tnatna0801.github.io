---
layout: article
title: 전화번호 목록
tag: 코테연습(프로그래머스)
---

## 해쉬

### 해쉬 개요

**배열**

* 장점 : index로 자료 검색이 이루어 지기 때문에 빠른 검색 속도
* 단점 : 삽입과 삭제가 어렵다.

**연결리스트**

* 장점 : 삽입과 삭제가 쉽다.
* 단점 : 탐색이 오래거린다.

한계를 극복하기 위해 나온 방법이 **해쉬(hash)**이다.

### 기본 개념

* 탐색 : O(1), 1번만에 접근하여 검색할 수 있음(대부분의 경우)
	* 내부적으로 배열을 사용하여 데이터를 저장하기 때문에 빠른 검색 속도를 갖는다.
* 해쉬 함수 : 데이터의 고유한 숫자값을 만들어 인덱스로 사용한다.
	* hash code
* 많은 값을 저장할 수 있는데 해쉬함수의 결과값이 적으면 충돌(collision)이 발생
	* 충돌 해결 방법 : 다음 빈칸에 저장, 데이블을 만들어 연결 리스트에 저장


## 전화번호 목록

### 문제
![전화번호목록](https://user-images.githubusercontent.com/48270067/90511264-2aa52a00-e197-11ea-81a9-51f443d12af1.png)

### 풀이

#### 1. 단순 반복
``` java
class Solution {
    public boolean solution(String[] phone_book) {
        for(int i = 0; i<phone_book.length-1; i++)
        {
            for(int j = i+1; j<phone_book.length; j++)
            {
                if(phone_book[i].startsWith(phone_book[j]))
                {return false;}
                if(phone_book[j].startsWith(phone_book[i]))
                {return false;}
            }
        }
        return true;
    }
}
```

* startsWith() : 비교 대상 문자열이 입력된 문자열 값으로 시작되는 지 여부를 확인하고 boolean 값으로 return하는 함수
* 이 함수를 알기 전에는 주어진 전화번호 중에서 가장 짧은 문자열을 for문으로 찾고 그걸 저장하고 근데 가장 짧은 문자열이 여러개일 수 있으니까 그걸 또 배열이나 linked list로 해야하나 생각했는데 역시 함수가 있었다.
* i와 j를 한번씩 비교 대상 문자열로 두어 탐색했다.
* 하지만 hash를 이용한 것은 아니기 때문에 패쓰

#### 2. hash
정해둔 시간안에 풀지 못해서 구글에 검색해보았다.

보고나니까 왜 이걸 생각해내지 못했나 싶고 열심히 살아야겠다.

1번 코드랑 거의 유사하다.

startsWith()함수를 사용하지 않고 기준 문자열의 길이를 저장해두고 비교 대상 문자열의 기준 문자열 길이 만큼만 비교하는 것이었다. 나는 이걸 생각해내지 못해서 앞에서 언급한 것처럼 for문을 하나 더 써서 접두어가 될만한 문자열부터 찾을 생각을 했다. 바보다바보

```java
import java.util.*;
class Solution {
    public boolean solution(String[] phone_book) {
        for(int i = 0; i<phone_book.length-1; i++)
        {
            int hashnum = phone_book[i].hashCode();
            int hashlen = phone_book[i].length();
                
            for(int j = i+1; j<phone_book.length; j++)
            {   
                if(phone_book[j].length() >= hashlen && hashnum == (phone_book[j].substring(0, hashlen).hashCode()))
                {return false;}
                else if(phone_book[j].length() < hashlen && (phone_book[i].substring(0, phone_book[j].length()).hashCode())  == phone_book[j].hashCode())
                {return false;}
            }
        }
        return true;
    }
}
}
```
* hashnum : 기준이 되는 전화번호의 hashcode를 저장하는 변수
* hashlen : 기준이 되는 전화번호의 길이를 저장하는 변수
* for문 두개 사용 : i는 기준이 되는 전화번호, j는 비교대상 전화번호
* if문 : 기준이 되는 전화번호가 비교대상 전화번호 보다 길이가 짧거나 같을 경우와 아닌경우로 나누어 연산 ==> 즉 기준이 되는 번호가 접두어가 될 수 있는지 먼저 검사
* 접두어와 비교대상의 hashCode가 일치하는지를 검사한다.



## 결과
![image](https://user-images.githubusercontent.com/48270067/90516364-d8680700-e19e-11ea-85fe-b9b2185d4f9f.png)
![image](https://user-images.githubusercontent.com/48270067/90516419-eb7ad700-e19e-11ea-9dd4-6f77ab971e0f.png)



## 회고
다음번엔 꼭 내가 풀어야지....ㅜ