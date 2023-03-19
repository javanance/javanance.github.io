---
description: >-
  최초인식시점의 계약(건별)단위의 보험계약마진을 결정하고 그에 따라 GoC를 분류한다. (최초인식시점의 CSM결정 및 손익구분 판단 후 건별
  GoC결정)
---

# New Contract

## class

{% hint style="info" %}
* SetupNcontCf  &#x20;계약단위 CF
* SetupNcontRst  &#x20;최초인식 대상의 CF, DF, RA, TVOG의 정보를 바탕으로&#x20;
  * 최초인식시점의  EPV, TVOM 산출
  * 손익구분 확정 및  최초CSM (or LOSS) 확정
* SetupGocCont  &#x20;계약 GoC mapping (PF, Cohort, 손익구분)
{% endhint %}

## job

| JOB | JOB\_DESC               | Method                        |
| --- | ----------------------- | ----------------------------- |
| N11 | NewCont 61회차 CF처리 & EPV | SetupNcontCf.createCurrent()) |
| N13 | NewCont Rst RA          | SetupNcontRst.createRa())     |
| N14 | NewCont Rst TVOG        | SetupNcontRst.createTvog())   |
| N15 | NewCont Rst EPV         | SetupNcontRst.createEpv())    |
| N16 | NewCont Rst Flat        | SetupNcontRst.createFlat())   |
| N17 | Map Cont Goc            | SetupGocCont.create())        |

## table

{% hint style="success" %}
* RawFv
* NcontCf
* NcontRstEpv
* NcontRstTvog
* NcontRstRa
* NcontRstFlat
{% endhint %}

