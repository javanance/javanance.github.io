---
description: Factory
---

# Bulid

각 로직에서 처리 완료한 데이터를 저장한다.&#x20;

{% hint style="info" %}
* FacNcont
* FacDfWghtRate
* FacCf  &#x20;/ FacDeltaCf
* FacCfDf
* FacRstEpv
* FacRstTvog
* FacRstRa
* FacRstOci
* FacBoxDetail  &#x20;/ FacBoxGoc
* FacRstCsm  &#x20;/ FacLossStep
* FacRstDac
* FacRollFwd  &#x20;/ FacRollFwdLoss
{% endhint %}

개별 작업단위에서 처리한 결과를 output 테이블 형태  bulid함.

builer pattern 으로 처리되어 Table에 적재되며. 이 output이 차기 단계 작업의 input이 됨.

* [ ] 읽어보고 지우기 : 팩토리 메서드 패턴은 공장의 개념을 구현하기 위한 객체지향 디자인 패턴이다. 다른 생성 패턴(creational patterns)들과 마찬가지로 이 패턴은 생성할 객체에 대한 정확한 클래스를 구분할 필요없이(알 필요없이) 객체를 생성하는 것에 대한 문제를 다룬다. 주로 객체를 생성할 때 보면, 그 객체를 결합하는(사용하는) 객체에서는 굳이 필요하지 않은 복잡한 과정이 필요하다. 객체의 생성은 심각한 코드의 중복을 야기할 수 있고, 결합하는(사용하는) 객체에서 접근할 수 없는 정보들을 요구할 수도 있으며, 충분한 추상화 단계를 제공하기 힘들 수도 있고, 결합하는(사용하는) 객체의 관심사에는 포함되지 않을 수도 있다. 팩토리 매서드 패턴은 객체를 생성할 때 별도의 메서드를 정의하고, 서브클래스에서 생성될 객체의 종류를 정할 수 있게 함으로써 이런 문제들을 해결한다.
