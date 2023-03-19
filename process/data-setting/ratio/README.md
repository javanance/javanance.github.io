---
description: GoC별 CSM 상각률 및 DAC 상각률을 관리
---

# RATIO

## class

{% hint style="info" %}
* SetupRatioCsm
* SetupRatioDac
{% endhint %}

## job

| JOB | JOB\_DESC         | Method                         |
| --- | ----------------- | ------------------------------ |
| M36 | DAC Ratio         | SetupRatioDac.createFromRaw(), |
| M39 | CSM Release Ratio | SetupRatioCsm.createFromRaw(), |

## table

{% hint style="success" %}
* RatioCovUnit
* RatioDac
{% endhint %}
