---
description: >-
  회계정보를 생성하기 위한 기본적인 데이터 가공작업을 수행한다. 아래의 작업은 GoC단위로 수행하며, 각 변동스텝별 예상 Cash Flow와
  실적 Cash Flow 금액을 집계하고, 평가에 사용되는 할인계수(Discount Factor)를 산출한다. 또한 CSM및 DAC 를
  상각하는데 사용할 상각률을 정리한다.
---

# Data setting

## class

{% hint style="info" %}
SETUP

* CF
* DF
* RA
* TVOG
* RATIO
* EPV NEW GoC&#x20;
{% endhint %}

## job

| JOB | JOB\_DESC                 | Method                             |
| --- | ------------------------- | ---------------------------------- |
| M21 | Real Cash                 | SetupCfLv3Cash.create(),           |
| M23 | Goc Current CF            | SetupCfLv1Goc.createCurrent(),     |
| M24 | Goc Movement CF           | SetupCfLv1Goc.createMovement())    |
| M25 | Goc 1st Newcont CF        | SetupCfLv1Goc.createFirstNew())    |
| M26 | Goc 2nd Newcont CF        | SetupCfLv1Goc.createSecondNew())   |
| M27 | Goc 3rd Newcont CF        | SetupCfLv1Goc.createThirdNew())    |
| M28 | Goc Previous CF           | SetupCfLv1Goc.createPrevClosing()) |
| M29 | Delta CF                  | SetupCfLv2Delta.create(),          |
| M31 | TVOG Lv1                  | SetupTvogLv1.create(),             |
| M32 | RA Lv1                    | SetupRaLv1.create(),               |
| M33 | TVOG Lv2                  | SetupTvogLv2Delta.create(),        |
| M34 | RA Lv2                    | SetupRaLv2Delta.create(),          |
| M35 | New Goc                   | SetupRstEpvNgoc.create(),          |
| M36 | DAC Ratio                 | SetupRatioDac.createFromRaw(),     |
| M39 | CSM Release Ratio         | SetupRatioCsm.createFromRaw(),     |
| M51 | Init Rate                 | SetupDfInitRate.createNew())       |
| M52 | Init Rate All for missing | SetupDfInitRate.createAll())       |
| M53 | Wghted Rate His           | SetupDfWgtRate.createNew(),        |
| M54 | Wghted His for missing    | SetupDfWgtRate.createAll())        |
| M55 | NewGoc EIR                | SetupDfEir.createNewgoc(),         |
| M56 | Curr EIR                  | SetupDfEir.create(),               |
| M57 | Wghted Rate Determined    | SetupDfWgtRate.determineNew())     |
| M58 | Wghted Rate for missing   | SetupDfWgtRate.determineAll())     |
| M59 | Goc Int Flat              | SetupDfLv3Flat.create(),           |
| M61 | Prev Cf + DF              | SetupCfLv4Df.createPrev(),         |
| M62 | Delta Cf + DF             | SetupCfLv4Df.createDelta())        |
| M63 | Curr Cf + DF              | SetupCfLv4Df.createCurr())         |
| M64 | Real CF                   | SetupCfLv4Df.createReal())         |
