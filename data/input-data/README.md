---
description: IFRS 17 의 회계정보를 생성하는 데 필요한 데이터 목록은 아래와 같이 구성됨.
---

# Input Data

이행 현금흐름을 구성하는 CF정보와 할인 및 금융비용 및 OCI를 산출하기 위한 DF정보를 구분하여 받는다. 또한 비금융위험에 대한 RA 및 TVOG역시 독립적으로 산출하는 경우 별도로 인터페이스 한다.&#x20;

{% hint style="info" %}
INPUT&#x20;

* **Raw**  &#x20;: 원천 data layer 외부 interface 대상&#x20;
* **Ncont**  &#x20;: 계약별 data (최초인식 시 건별 GoC 결정)
* **Cf** : Fulfilment cash flow 명목금액&#x20;
* **Df** : 할인계수 (최초, 직전, 현행, 체계적배분 등 필요 할인계수)
* **TVOG / RA** : TVOG, RA를 별도의 session에서 산출하는 경우&#x20;
* **Ratio**  &#x20;: GoC별 CSM상각률 , DAC 상각률
{% endhint %}

{% hint style="warning" %}
**check !!**&#x20;

* GoC
  * mst , 상각률, real, RA, TVOG , 전환시점 CSM,LOSS, DAC 잔액, 실적data
* CF max bucket (`MAX_TENOR`) != DF max bucket (`MAX_RATE_TENOR`)&#x20;
  * 현금흐름 프로젝션 기간보다 할인율 커브가 짧은 경우 평가가치에 오차 발생할 수 있음&#x20;
* CF
  * 전기 결산일 Data (직전결산 기말부채 평가 : 당결산 기시부채)
  * 당기 결산일 Data (변동분석 및 기말부채평가 )
* DF
  * 직전 / 당기 / 최초인식시점 누락된 월도 없는지&#x20;
  * 신계약 최초인식월 할인율 커브 (Properties 설정과 데이터 기준이 같은지 )
  * 전환시점 및 신계약 GoC의 산출된  EIR 이 0 이 아닌지 : EIR 산출값 검증 후 작업 !  (FCOST / AOCI) (확인 대상 : EIR, NEWGOC\_EIR)
    * NEWGOC\_EIR : CS (S가 0인 경우 확인하기)
  * &#x20;CSM INT = 0 wght\_rate 확인
{% endhint %}



