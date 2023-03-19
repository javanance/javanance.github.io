---
description: Data Access Object
---

# DAO

각 처리 로직에서 필요한 데이터를 가져온다.&#x20;

{% hint style="info" %}
* RawDao  &#x20;: 원천 data layer
* NcontDao  &#x20;: 계약별 data (최초인식 시 건별 GoC 결정)
* CfDao  &#x20;: Fulfilment cash flow 명목금액&#x20;
* DfDao  &#x20;: 할인계수 (최초, 직전, 현행, 체계적배분 등 필요 할인계수)
* TvogDao  &#x20;: TVOG를 별도의 session에서 산출하는 경우&#x20;
* RaDao  &#x20;: RA를 별도의 session에서 산출하는 경우&#x20;
* RatioDao  &#x20;: GoC별 CSM상각률 , DAC 상각률
* MapDao  &#x20;: Mapping 관계&#x20;
* MstDao  &#x20;: Master 속성 정보
* BoxDao  &#x20;: BOX Result&#x20;
* RstDao  &#x20;: Result&#x20;
{% endhint %}

GMV의 JOB 은 선/후행 작업에 따라 직전 JOB의 결과를 기준으로 전진적으로 처리.&#x20;

개별 작업단위에서 처리한 결과가 Table에 적재되며. 단계별 수행과정에서 이전 단계의 결과값을 가져오는 경우 직전의 output이 차기 단계 작업의 input이 됨.

GMV에서 사용되는 Table은 entity에서 기술함.

DAO는 JOB에서 수행할 로직처리를 위해 이전 JOB에서 처리한 결과를 entity에서 필요한 형태로 가져오는 역할을 함.



