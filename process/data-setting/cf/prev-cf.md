---
description: 직전 현금흐름
---

# M25\~M28 : Previous CF

### FROM

* &#x20;MST\_RUNSET
* RAW\_GOC\_CF

### TO&#x20;

* &#x20;CF\_LV1\_GOC

### DO

전기 결산 현금흐름 + 결산단위 내 신규물량 (New Business) 을 직전 현금으로 정의.&#x20;

> #### 제공한 서비스 타이밍의 식별&#x20;

직전현금흐름을 기준으로 당기 및 과거 관련 서비스를 식별함. Release의 효과 (의무 이행으로 발생하는 CF Release, 시간의 경과(Time Release)로 인식하는 이자비용 )는 직전 결산에 쓰인 현금흐름 뿐만 아니라, 결산기간 내 신규물량에서도 발생하는 개념이기 때문에 GMV는 전기 결산의 이행 현금흐름과 신규물량의 이행 현금흐름을 Prev CF로 구분하여 처리함.

> #### 결산주기 ≠ 예상현금흐름 산출주기

예상현금의 산출주기와 결산 주기가 동일하지 않은 경우 (Example :  예상현금흐름 M(월)산출/ 결산주기 : Q(분기)결산)의 경우, 해당 분기 결산 시, 신계약 물량이 최대 3개월에 걸쳐 유입될 수 있음. 매월 유입된 신계약의 최초인식시점과 결산시점의 차이가 상이하므로 결산분기 내 몇 번째 월에 유입된 신규 물량인지 구분(1st, 2nd, 3rd)하여 처리함.

> #### **경과회차 조정**&#x20;

변동분석을 위해 직전 결산CF를 기시 잔액으로 간주하고, 결산기간 중 변동 flow를 순차로 인식하며 변동분을 인식함. 이때 사용되는 직전 결산 CF 및 신계약 CF는  직전 기준(신계약은 신계약 인식 시점 기준)으로 생성된 정보이므로 당기 말 결산 시점 기준의 CF와 비교할 때 동일 시점으로 회차 조정이 필요함.&#x20;

경과회차 조정은 현 결산일에 사용할 직전결산 CF 및 분기결산 사이의 신규물량 CF의 경과회차를 현 당기말 결산 시점의 회차로 기준을 맞추는 작업을 의미한다.&#x20;

### DESC

*
*

### SRC

| JOB | JOB\_DESC          | Method                             |
| --- | ------------------ | ---------------------------------- |
| M25 | Goc 1st Newcont CF | SetupCfLv1Goc.createFirstNew())    |
| M26 | Goc 2nd Newcont CF | SetupCfLv1Goc.createSecondNew())   |
| M27 | Goc 3rd Newcont CF | SetupCfLv1Goc.createThirdNew())    |
| M28 | Goc Previous CF    | SetupCfLv1Goc.createPrevClosing()) |
