---
layout: article
title: 2020 하계 모각코 1회 회고록
aside:
  toc: true
tag: 2020_하계모각코
---

# 1회차 : 2020/07/16

## 목표

* Microsoft Azure chatbot
* dispatch model 만들기
* C#

## 결과

Core bot에 LUIS 모델과 QnA 모델을 서비스하기 위해서 방법을 찾던 중 Dispatch를 알게 되었다.

### 단계

1. corebot 생성

2. LUIS와 QnA model 생성

3. dispatch model 생성
``` bash
# core bot의 CognitiveModels 디렉토리 안에서 수행
# dispatch 설치 도구 확인
npm i -g npm
npm i -g botdispatch
```
``` bash
# dispatch 파일 초기화
dispatch init -n <filename-to-create> --luisAuthoringKey "<your-luis-authoring-key>" --luisAuthoringRegion <your-region>
```
``` bash
#LUIS, QnA 추가
dispatch add -t luis -i "<app-id-for-home-automation-app>" -n "<name-of-home-automation-app>" -v <app-version-number> -k "<your-luis-authoring-key>" --intentName l_HomeAutomation
 dispatch add -t qna -i "<knowledge-base-id>" -n "<knowledge-base-name>" -k "<azure-qna-service-key1>" --intentName q_sample-qna
```

4. code 수정 : Visual Studio
	* appsettings.json
	* BotService.cs
	* IBotService.cs

5. deploy dispatch chatbot

### 결과
<img width="544" alt="1_success" src="https://user-images.githubusercontent.com/48270067/89640436-8398d680-d8ea-11ea-9961-1eeeeba1bdab.PNG">
* QnA model만 서비스했을 때의 챗봇이다. 정상적으로 답을 준다.

<img width="534" alt="1_error" src="https://user-images.githubusercontent.com/48270067/89640683-0d48a400-d8eb-11ea-90a3-3620f55abbdb.PNG">
* dipatch를 적용한 모습니다. 정상적으로 작동하지 않는 것을 확인할 수 있다. 어떤 부분에서 오류가 나는지 확인하기 위해 출력해보았다. 사용자의 입력을 분류를 못하고 바로 오류문을 띄워주는 것을 확인했다.

* 다음번에 이부분을 바꾸면서 다른 기능도 추가해야겠다.


## 느낀점

적용을 하려고했는데 처음 접하는 내용이라 쉽지 않았다. [참고자료](https://docs.microsoft.com/ko-kr/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=cs)를 참고해서 진행하였는데 LUIS model인지 QnA model인지 인지하지 못하고 자꾸 NONE이라고 출력된다.

결국 모각코 시간내에 해결하지 못하여 아쉬웠다.