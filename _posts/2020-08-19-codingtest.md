---
layout: article
title: 위장
tag: 코테연습(프로그래머스)
---

## HashMap
["key", "value"]

* key는 중복 x
* value는 중복 o


## 위장

### 문제 설명
스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

|종류|이름|
|---|---|
|얼굴|동그란 안경, 검정 선글라스|
|상의|파란색 티셔츠|
|하의|청바지|
|겉옷|긴 코트|
스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

#### 제한사항
* clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
* 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
* 같은 이름을 가진 의상은 존재하지 않습니다.
* clothes의 모든 원소는 문자열로 이루어져 있습니다.
* 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 _ 로만 이루어져 있습니다.
* 스파이는 하루에 최소 한 개의 의상은 입습니다.

### 풀이
처음에 의상이름을 key로 하고 의상 종류를 value로 해서 문제를 풀려고 시도했다.

하지만 이 방법을 사용하면 for문을 너무 많이 사용할 것같아 아래 방법으로 코드를 짰다.

```java
import java.util.HashMap;
class Solution {
    public int solution(String[][] clothes) {
        int answer = 1;
        
        HashMap<String, Integer> cm = new HashMap<>();
    
        for(int i = 0; i < clothes.length; i++){
            if(cm.containsKey(clothes[i][1]))
                cm.replace(clothes[i][1], cm.get(clothes[i][1]) + 1);
            else
                cm.put(clothes[i][1], 1);
        }
    
        for (int value : cm.values()) {
            answer *= (value + 1);
        }
    
        answer -= 1;
        return answer;
    }
}
```

* key를 의상 종류로하고 value를 갯수로 하는 hashmap을 만든다.
* 경우의 수 : (a 의상 종류의 개수 + 1(안 입는 경우)) x (b 의상 종류의 개수 + 1(안 입는 경우)) x ... x (n 의상 종류의 개수 + 1(안 입는 경우))
* 첫번째 for문으로 hashmap을 초기화한다. if문으로 이미 hashmap에 key가 존재하면 value + 1을 해준다.
* 두번째 for문으로 경우의 수를 계산한다.
* 마지막에 answer - 1하는 이유는 모든 의상을 안입는 경우를 제외하기 위함이다.

## 회고
카테고리가 hash가 아니었다면 hash를 금방 떠올리지 못햇을 것같다.