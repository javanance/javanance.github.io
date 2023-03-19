---
description: TVOG와 RA 산출 결과를 movement 분석형태로 가져옴
---

# TVOG & RA

## class

{% hint style="info" %}
* SetupTvogLv1
* SetupTvogLv2Delta
* SetupRaLv1
* SetupRaLv2Delta
{% endhint %}

## job

| JOB | JOB\_DESC | Method                     |
| --- | --------- | -------------------------- |
| M31 | TVOG Lv1  | SetupTvogLv1.create()      |
| M32 | RA Lv1    | SetupRaLv1.create()        |
| M33 | TVOG Lv2  | SetupTvogLv2Delta.create() |
| M34 | RA Lv2    | SetupRaLv2Delta.create()   |

## table

{% hint style="success" %}
* TvogLv1
* TvogLv2Delta
* RaLv1
* RaLv2Delta
{% endhint %}
