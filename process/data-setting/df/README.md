---
description: Discount Factor에서는 IFRS 17 회계항목 처리에 필요한 할인계수를 생성. (현행 , 직전,  최초, 체계적배분)
---

# DF

GoC(PF)별 / oci정책 / 회계모형&#x20;

* [ ] 최초할인율 option (GoC 최초인식일의 할인율사용 , 가중평균하여 사용 )
* [ ] GoC 최초 할인율 (가중평균) , open cohort 신규물량 가중치 기준&#x20;
* [ ] GoC 최초 할인율 (가중평균) history관리체계
* [ ] option 금리개정주기 (직전 결산 vs 매달)
* [ ] 회계모형에 따른 체계적배분율 (Non-par / In-par) : 최초인식시점의 기초자산무관 명목 / 단일율
* [ ] In-par 체계적배분율(단일률;EIR) 산출로직 : Option EIR-SL(Start-Lockin) 조정분을 OCI로 처리하는 경우 vs PL로 처리하는 경우&#x20;

## class

{% hint style="info" %}
* SetupDfCurrRate
* SetupDfInitRate
* SetupDfWgtRate
* SetupDfEir
* SetupDfLv3Flat
{% endhint %}

## job

| JOB | JOB\_DESC                 | Method                         |
| --- | ------------------------- | ------------------------------ |
| M51 | Init Rate                 | SetupDfInitRate.createNew())   |
| M52 | Init Rate All for missing | SetupDfInitRate.createAll())   |
| M53 | Wghted Rate His           | SetupDfWgtRate.createNew(),    |
| M54 | Wghted His for missing    | SetupDfWgtRate.createAll())    |
| M55 | NewGoc EIR                | SetupDfEir.createNewgoc(),     |
| M56 | Curr EIR                  | SetupDfEir.create(),           |
| M57 | Wghted Rate Determined    | SetupDfWgtRate.determineNew()) |
| M58 | Wghted Rate for missing   | SetupDfWgtRate.determineAll()) |
| M59 | Goc Int Flat              | SetupDfLv3Flat.create(),       |

## table

{% hint style="success" %}
* DfLv1CurrRate
* DfLv2Eir
* DfLv2EirNewgoc
* DfLv2InitRate
* DfLv2WghtHis
* DfLv2WghtRate
* DfLv3Flat
{% endhint %}
