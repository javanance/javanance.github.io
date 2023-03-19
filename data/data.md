---
description: 'Table : Entity'
---

# Data Mart 개요

IFRS 17의 회계정책에 따른 회계정보생성 및 분개 처리를 위해 필요한 Data Mart .  크게 Input , Output ,  master 로 구성됨.&#x20;

{% hint style="info" %}
entity의 특성으로 구별 시 크게 아래와 같음.

* INPUT&#x20;
  * **Raw**    &#x20;: 원천 data layer 외부 interface 대상&#x20;
  * **Ncont**    &#x20;: 계약별 data (최초인식 시 건별 GoC 결정)
  * **Cf** : Fulfilment cash flow 명목금액&#x20;
  * **Df** : 할인계수 (최초, 직전, 현행, 체계적배분 등 필요 할인계수)
  * **TVOG / RA** : TVOG, RA를 별도의 session에서 산출하는 경우&#x20;
  * **Ratio**    &#x20;: GoC별 CSM상각률 , DAC 상각률
* OUTPUT
  * **Box**    &#x20;: BOX Result&#x20;
  * **Rst**    &#x20;: Result&#x20;
* MASTER
  * **Map**    &#x20;: Mapping 관계&#x20;
  * **Mst**    &#x20;: Master 속성 정보
{% endhint %}

