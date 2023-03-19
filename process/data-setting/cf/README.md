---
description: 이행현금흐름
---

# CF

IFRS 17 결산정보를 생성하기 위해 필요한 현금흐름 항목을 정의하고, 변동분석에 용이하도록 CF를 가공하는 프로세스에 대해 설명한다.

변동분석의 순서 및 변동스텝 세분화는 정해져있지 않. 변동 효과를 구분하여 인식하고 싶은 수준으로 정의하며,  변동효과를 구분하는 기준의 현금흐름 산출 실행세트 (Runset이라고 칭함) 단위의 현금흐름을 필요로 함.&#x20;

할인계수를 곱하여 산출된 부채평가액 :BEL (Best Estimated Liability) 이 아닌 이행현금흐름 형태로 미래 발생가능한 사건이 현행위험률 및 현행가정치를 반영하여 확률가중치로 평가된 할인되지 않은 명목금액 형태의 경과회차 별 Cash Flow를 Input 항목의 CF로 정의함.

{% hint style="success" %}
**RollFwd step에 따른 필요 CF 구분**&#x20;

* **Previous (직전) 결산시점**&#x20;
*   직전시점 현금흐름은 직전 결산용 CF와 신계약 CF로 구성됨&#x20;

    * 직전 결산 이행현금흐름 (전기 현금흐름)
    * 신계약 이행현금흐름 (인식시점별)


* **Current (당기) 결산시점**&#x20;
* 결산시점 현금흐름은 movement CF와 당기 결산용 Current CF 로 구성됨&#x20;
  * 공시이율변동&#x20;
  * 물량변동&#x20;
  * 계리적가정변동
  * ...
  * 금융가정변동
  * 재량가정변동
  * 결산 이행현금흐름 (마지막 변동분석 스텝의 결과와 동일)&#x20;
{% endhint %}

## 주요 처리내용&#x20;

> #### **코드 변환**

Raw data의 코드 값이 아닌 GMV시스템 내부 코드로 변환하는 작업을 의미한다. 내부적인 일관성을 가지고 코드값을 정의하여 변환되며 매핑관계 및 코드의 유효값 및 관리방안은 Master setting 의 map에서 다룬다.&#x20;

> #### **마지막 CF 회차 조정 (Optional)**

raw CF 출력 시, 용량 및 환경적 제약으로 Life time 까지 출력이 어려운 경우 현금흐름 관측기간을 제한적으로 출력하는 경우에 마지막 버킷에 합계액으로 출력하여 표현된 현금흐름의 회차를 조정하는 작업을 의미한다. 마지막 버킷의 회차에 현가(PV) 와 명목금액 및 사용된 할인율 커브를 이용하여 유동성 만기를 고려하여 경과회차를 조정하는 처리를 수행한다. &#x20;

## class

{% hint style="info" %}
* SetupCfLv1Goc
* SetupCfLv2Delta
* SetupCfLv3Cash
* SetupCfLv4Df
{% endhint %}

## job

| JOB      | Desc                                                  | Class                                          |
| -------- | ----------------------------------------------------- | ---------------------------------------------- |
| M21\~M29 | GoC CF (real cash, Current, Movement, Newcont, Delta) | SetupCfLv1Goc, SetupCfLv3Cash, SetupCfLv2Delta |
| M61\~M64 | CF + DF (Prev, Delta, Curr, Real)                     | SetupCfLv4Df                                   |

| JOB     | JOB\_DESC          | Method                             |
| ------- | ------------------ | ---------------------------------- |
| M21     | Real Cash          | SetupCfLv3Cash.create(),           |
| M23     | Goc Current CF     | SetupCfLv1Goc.createCurrent(),     |
| M24     | Goc Movement CF    | SetupCfLv1Goc.createMovement())    |
| M25     | Goc 1st Newcont CF | SetupCfLv1Goc.createFirstNew())    |
| M26     | Goc 2nd Newcont CF | SetupCfLv1Goc.createSecondNew())   |
| M27     | Goc 3rd Newcont CF | SetupCfLv1Goc.createThirdNew())    |
| M28     | Goc Previous CF    | SetupCfLv1Goc.createPrevClosing()) |
| ~~M29~~ | ~~Delta CF~~       | ~~SetupCfLv2Delta.create()~~       |
| M61     | Prev Cf + DF       | SetupCfLv4Df.createPrev()          |
| M62     | Delta Cf + DF      | SetupCfLv4Df.createDelta())        |
| M63     | Curr Cf + DF       | SetupCfLv4Df.createCurr())         |
| M64     | Real CF            | SetupCfLv4Df.createReal())         |

## table

{% hint style="success" %}
* CfLv1Goc
* CfLv2Delta
* CfLv3Real
* CfLv4Df
{% endhint %}

