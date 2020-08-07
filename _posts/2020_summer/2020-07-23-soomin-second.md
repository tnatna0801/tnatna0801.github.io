---
layout: article
title: [개별] 2020 하계 모각코 2회 회고록
aside:
  toc: true
tag: 2020_하계모각코
---

# 2회차 회고록

## 목표

* HEAP 구조 정리

## 결과
![2-1](https://user-images.githubusercontent.com/48270067/89640421-7e3b8c00-d8ea-11ea-840d-c28a3361d0fb.jpg)
![2-2](https://user-images.githubusercontent.com/48270067/89640427-80054f80-d8ea-11ea-860b-1a9b1a4a9cef.jpg)
![2-3](https://user-images.githubusercontent.com/48270067/89640429-81367c80-d8ea-11ea-80fd-80f8d75a85ac.jpg)

## 회고

free()를 호출한 후 메모리를 확인했는데 예상한 결과가 안나와서 당황했다. 코드에 문제가 있나부터 data 주소를 잘못알고있나까지 생각이 많았는데 알고보니 tcache bin 때문이었다. 이부분의 내용은 아직 이해를 다 못해서 적지 않았다. 다음시간에 이어서 알아볼 예정이다.