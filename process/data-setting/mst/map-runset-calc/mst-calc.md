# MST CALC

## CALC\_TYPE&#x20;

| CALC  TYPE | CONTENT                                            |
| ---------- | -------------------------------------------------- |
| BOX        | CF에 곱할 할인계수를 BOX ID로 정의하여 곱으로 산                    |
| REF        | CALC\_ID에 기 정의된 항목에 추가로 비율을 곱하여 산                  |
| NATIVE     | 산출방식이 이미 확정된 항목은 NATIVE로 분류하며, CALC METHOD에 따라 세분  |
| WATERFALL  | 순차적으로 산출되는 항목, 직전 단계의 산출결과를 기반으로 산출되는 항목           |
| DELTA SUM  | 이전 단계까지의 누적 합계액 (정보성) Close step에서만 출력             |

#### REF

손실요소 배부대상과 같이 CALC\_ID로 기 정의된 (금융비용 및 당기 서비스 제공분 확정된 대상) 산출 결과를 바탕으로 일정 비율(LOSS\_ADJUST)을 곱하여 금액을 확정하는 경우 REF방식으로 정의함.

| CALC\_ID            | CALC\_NAME           | SERVICE\_TIMING | REF\_CALC\_ID    |
| ------------------- | -------------------- | --------------- | ---------------- |
| BOX\_Cp\_L2         | 금융비용 (손실요소 Release)  | ALL             | BOX\_Cp\_LOSS    |
| BOX\_Up\_L2         | 최초인식 LOSS\_ALLO      | ALL             | BOX\_Up\_LOSS    |
| BOX\_U0\_L2         | 최초인식 LOSS\_ALLO      | ALL             | BOX\_U0\_LOSS    |
| BOX\_ZU\_L2         | 예상 CF (손실요소 Release) | CURRENT         | BOX\_ZU\_LOSS    |
| RA\_INIT\_CLOSE\_L2 | 최초인식 LOSS\_ALLO      | ALL             | RA\_INIT\_CLOSE  |
| RA\_RELEASE\_L2     | RA 상각 (손실요소 Release) | ALL             | RA\_RELEASE      |
| BOX\_U0\_L2\_C      | 당기 CF(손실요소 Release)  | CURRENT         | BOX\_U0\_LOSS\_C |

#### WATERFALL

| CALC\_ID        | CALC\_NAME        | SIGN\_ADJUST |
| --------------- | ----------------- | ------------ |
| RA\_PREV        | 기시 RA             | PLUS         |
| RA\_INIT        | 최초인식 (RA)         | PLUS         |
| RA\_INIT\_CLOSE | RA 최초인식 집계        | PLUS         |
| RA\_RELEASE     | RA 상각 (보험수익)      | MINUS        |
| RA\_CHANGE      | delta RA변동액 (CSM) | PLUS         |
| RA\_CURR        | 기말 RA             | PLUS         |
| TVOG\_PREV      | 기시 TVOG           | PLUS         |
| TVOG\_INIT      | 최초인식 TVOG         | PLUS         |
| TVOG\_CHANGE    | TVOG 변동           | PLUS         |
| TVOG\_CURR      | 기말 TVOG           | PLUS         |

#### DELTA SUM

| CALC\_ID           | CALC\_NAME         |
| ------------------ | ------------------ |
| CSM\_CLOSE         | 기말 CSM (장부)        |
| DAC\_CLOSE         | 기말 DAC             |
| CLOSE\_STEP        | GENERAL CLOSE STEP |
| RA\_SERVICE\_CLOSE | RA SERVICE STEP    |

## CALC METHOD

`CALC_TYPE = NATIVE`인 경우에만 CALC\_METHOD를 정의하며  대상은 산출방식이 이미 정의된 항목에 한정됨. 예를들어 CSM 부리이자와 같이 사용자가 산출방식을 회계정책에 따라 산식을 정의할 수 있는 항목이 아니라, 기준서 상 정의된 방식으로 산출해야 하는 경우의 항목 산출에 해당함.&#x20;

| CALC TYPE | CALC\_METHOD           |
| --------- | ---------------------- |
| NATIVE    | CSM\_INIT              |
| NATIVE    | CSM\_INT               |
| NATIVE    | CSM\_LOSS\_ALLO        |
| NATIVE    | CSM\_PREV              |
| NATIVE    | CSM\_RELEASE\_1        |
| NATIVE    | CSM\_RESERVE           |
| NATIVE    | CSM\_REVERSAL          |
| NATIVE    | CURR\_LOSS\_CLOSE      |
| NATIVE    | DAC\_PREV              |
| NATIVE    | DAC\_RELEASE\_1        |
| NATIVE    | DELTA\_LOSS\_ALLO      |
| NATIVE    | LOSS\_FACE\_PREV\_ALLO |
| NATIVE    | LOSS\_PREV\_ALLO       |
| NATIVE    | LOSS\_RA\_PREV\_ALLO   |
| NATIVE    | LOSS\_TVOM\_PREV\_ALLO |
| NATIVE    | RA\_INT                |
| NATIVE    | TVOG\_INT              |
