---
description: MdlRstBox.createRa()
---

# Box Result

## class

{% hint style="info" %}
* MdlRstBox
* MdlRstCsm
* MdlLossStep
* MdlRstDac
{% endhint %}

{% hint style="warning" %}
**BOX Result** (전환)

* MdlInitCsm
* MdlInitLoss
* MldFvFlat
{% endhint %}

## job

| JOB | JOB\_DESC            | Method                   |
| --- | -------------------- | ------------------------ |
| M71 | Box CF Detail        | MdlRstBox.createDetail() |
| M72 | Box Ra               | MdlRstBox.createRa()     |
| M73 | Box TVOG             | MdlRstBox.createTvog()   |
| M75 | Box CF GOC           | MdlRstBox.createGoc()    |
| M81 | Fulfill at Loss Step | MdlLossStep.create()     |
| M82 | Box Rst              | MdlRstCsm.create()       |
| M86 | DAC Calc             | MdlRstDac.create()       |

## table

{% hint style="success" %}
* RstBoxDetail
* RstBoxGoc
* RstLossStep
* RstCsm
* RstDac
{% endhint %}
