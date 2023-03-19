---
description: Job이란 GMV에서 처리하는 작업의 세부단위를 의미한다. 개별 Process를 Job 단위로 구분하여 전진적으로 처리한다.
---

# Job

전환시점 최초로 수행해야 하는 작업은 C(conversion)로 구분하며, 정규작업은 M (main) 으로 구분한다. 신규 물량 유입 시 계약 별 (증권번호) GoC를 확정짓는 작업은 N (new contract) 으로 구분한다. 또한 특정 GoC만 선택하여 작업을 수행하고 싶은 경우 G(GoC)로 구분된 작업을 수행한다.&#x20;

정규 결산은  GoC단위에서 처리하는 Data setting ( CF 가공, GoC별 DF (필요 할인계수) 생성), Box 결과 생성, 회계정보 생성으로 구성된다.&#x20;

신규 물량의 경우 계약(증권번호)별 최초인식시점에 손실부담계약여부 판단 및 최초할인율 결정 등 계약 단위에서 결정하는 항목을 처리한다. 최초인식시점에 측정한 이행현금흐름 및 RA, TVOG금액을 기준으로 최초인식시점의 계약별 CSM (혹은 Loss)을 확정하고, 그에 따라 GoC를 분류하는 작업이 수행된다.&#x20;

> **정규결산 (계약별 처리)**&#x20;

| JOB      | Desc                                 | Class                                     |
| -------- | ------------------------------------ | ----------------------------------------- |
| N11\~N17 | NewContract EPV, RA, TVOG, Flat ,GoC | SetupNcontCf, SetupNcontRst, SetupGocCont |



> **정규결산 (GoC별 처리)**

| JOB      | Desc                                                  | Class                                                                       |
| -------- | ----------------------------------------------------- | --------------------------------------------------------------------------- |
| M01\~M02 | Curr Rate                                             | SetupDfCurrRate                                                             |
| M21\~M29 | GoC CF (real cash, Current, Movement, Newcont, Delta) | SetupCfLv1Goc, SetupCfLv3Cash, SetupCfLv2Delta                              |
| M31\~M34 | TVOG, RA                                              | SetupTvogLv1, SetupTvogLv2Delta, SetupRaLv1, SetupRaLv2Delta                |
| M35      | New Goc의 EPV                                          | SetupRstEpvNgoc                                                             |
| M36\~M39 | Ratio (DAC, CSM release)                              | SetupRatioDac, SetupRatioCsm                                                |
| M51\~M59 | DF (Init, Wghted Init, EIR, Flat)                     | SetupDfInitRate, SetupDfWgtRate, SetupDfEir, SetupDfWgtRate, SetupDfLv3Flat |
| M61\~M64 | CF + DF (Prev, Delta, Curr, Real)                     | SetupCfLv4Df                                                                |
| M71\~M75 | Box result (CF Detail, RA, TVOG, CF GoC)              | MdlRstBox                                                                   |
| M81\~M86 | result (Loss distribution ratio, CSM, DAC)            | MdlLossStep, MdlRstCsm, MdlRstDac                                           |
| M94\~M97 | Roll Fwd (fullfilment CF, CSM, DAC, Loss)             | MdlRollFwd, MdlRollFwdLoss                                                  |
| M98\~M99 | Journal Account                                       | MdlAcctBox                                                                  |



전환시점에 처리할 사항은 GoC별 FV(공정가치)와  EPV(FACE, TVOM), TVOG , RA 를 비교하여, 전환 시 CSM 및 LOSS금액을 확정하고, 전환시점의 DAC잔액을 확정짓는다. 또한 전환 당시 보유하고 있는 GoC의 최초할인율, 체계적 배분에 사용할 단일률(EIR)을 산출한다.&#x20;

> **전환 작업 (GoC별 처리)**

| JOB | JOB\_DESC                | Method                               |
| --- | ------------------------ | ------------------------------------ |
| C12 | NewCont 61회차 CF 처리 & EPV | SetupNcontCf.createAll())            |
| C24 | Goc Current CF           | SetupCfLv1Goc.createConversion())    |
| C30 | FairValue                | MldFvFlat.createConversion())        |
| C31 | TVOG RST                 | SetupTvogLv1.createConversion())     |
| C32 | RA RST                   | SetupRaLv1.createConversion())       |
| C35 | Curr EPV                 | SetupRstEpvNgoc.createConversion())  |
| C51 | Init Rate                | SetupDfInitRate.createAll())         |
| C53 | Weighted Rate Historical | SetupDfWgtRate.createConversion())   |
| C55 | Weigthed Rate Determined | SetupDfWgtRate.determineAll())       |
| C57 | NewGoc Eir at conversion | SetupDfEir.createNewGocConversion()) |
| C58 | EIR at conversion        | SetupDfEir.createConversion())       |
| C63 | CF +DF : Curr Conversion | SetupCfLv4Df.createConversion())     |
| C81 | Fulfill at Loss Step     | MdlLossStep.createConversion())      |
| C82 | Init Csm                 | MdlInitCsm.createConversion())       |
| C86 | Init DAC                 | MdlRstDac.createConversion())        |
