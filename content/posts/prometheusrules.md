---
title: prometheus-operator 의 defaultRules
date: 2021-02-04
categories: prometheus kubernetes
---

[prometheus-community/kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) 차트를 사용하면, defaultRules 가 굉장히 많이 켜져 있는데, 이게 편리하달지 모르고 쓰는 무기랄지 모르겠음.

검증용 도구로써 활용하면 좋을 것 같긴 함.

1. 일단 다 켜두고 alert 이 오는지 확인.
2. 근데 다 켜두고 alert 을 제대로 오게 하는 것도 볼게 많은게 함정.
3. 다 켰으면 발생하는 alert 확인하고 뭐가 제대로 동작하지 않는 건지 원인파악해서 해결.

3번 까지 마쳤으면 뭔가 상태가 이상하면 알아서 잘 alert 이 오는 건 좋긴한데..
아 이제 된건가? 하고 안심할 수도 없다.
defaultRules 에 얼마나 많은 alert 이 설정됐고 각각의 rule 이 어떤 상황을 설명하는건지 모르거던.
커뮤니티에서 알아서 잘 해줬겠지.. 믿고 가고 싶지만 아 이제 alert 이 안오는 걸 보니 제대로 됐군 이라는 안정감보다는 불명확한 구성을 인스톨했다는데서 오는 불안감이 있음.

차트로써 인스톨하는 것들이 다 그러함.
차트제작자님 믿습니다. :pray: 랄까.

클라우드에서는 모르겠는데 on-premise 에서는 default 로 설치했다? -> 뭐가 잘 안됨이라서..

metric 이 잘 수집되게 하는데만해도 이것저것 살필게 많음.

- kube component 들이 auto-discovery 가 안되고,
- 컴포넌트 metric 수집 endpoint 에 tls 를 사용했다면 인증 문제를 해결해야 하고,
- 각 컴포넌트들의 exporter 에 버그들이 존재하는데 살펴보면 해결하는 중이고, (이런 건 패치할 능력이 안되니 alert off)
- component 를 실행시키는 옵션에 따라 수집되고 안되고,
- ...

alert 이 발생해도 피곤한 것이 어느 rule 의 어느 expression 이 무슨역할을 하는지 왜 alert 이 발생했는지 살펴봐야하고,
문제를 트러블슈팅하는 것도 상황이 넘 다양해서 어려웡..

수집되는 metric 이 엄청나게 많고 누가 뭘 쓰고 이 metric 은 뭐고를 모르는 상황.
근데 alert 이 발생. 반복.
뭐지? 하고 넘기면 이건 alert 붙여놓은거나 마나인거고(쿨타임 찰때마다 불멸의 alert 을 받게됨)

각각의 지표가 무슨 역할을 하는건지 알아야 하고, alert 을 발생시킨 prometheus rule 을 알아야 하는데,
그걸 어케 다 들여다보지. 이거는 클러스터 개발/운영의 작은 부분집합일 뿐인데.

남들은 어케 이걸 다 하고 있는건가 싶기도 하고 존경스러움.

오픈소스 형님들 믿습니다.

또 똥글을..

뭔가 실용적인 내용을 써보고 싶은데 힝..
