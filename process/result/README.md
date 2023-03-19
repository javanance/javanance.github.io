# Account Result

## class

{% hint style="info" %}
* MdlRollFwd
* MdlRollFwdLoss
* MdlAcctBox
{% endhint %}

## job

| JOB | JOB\_DESC       | Method                              |
| --- | --------------- | ----------------------------------- |
| M94 | Roll Fwd        | MdlRollFwd.create()                 |
| M95 | Roll Fwd        | MdlRollFwd.createCsm()              |
| M96 | Roll Fwd        | MdlRollFwd.createDac()              |
| M97 | Roll Fwd        | MdlRollFwdLoss.create()             |
| M98 | Journal Account | MdlAcctBox.createFromRollFwd()      |
| M99 | Journal Account | MdlAcctBox.createFromRollFwdLoss()) |

## table

{% hint style="success" %}
* RstRollFwd
* RstRollFwdLoss
* RstLossStep
* RstCsm
* RstDac
* AcctBox
* AcctBoxGoc
{% endhint %}

