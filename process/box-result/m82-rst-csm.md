---
description: MdlRstCsm.create()
---

# M82 : RST BOX CSM

### FROM

* RST\_BOX\_DETAIL&#x20;

### TO&#x20;

* RST\_BOX

### DESC

*
*

### SRC

{% tabs %}
{% tab title="create()" %}
{% code title="MdlRstCsm.java" %}
```java
	public static Stream<RstCsm> create() {
		List<RstCsm> rstList = new ArrayList<RstCsm>();
		for (String gocId : PrvdMst.getGocIdList()) {
			rstList.addAll(create(gocId).collect(toList()));
		}
		return rstList.stream();
	}
```
{% endcode %}
{% endtab %}

{% tab title="create(String gocId)" %}
```java
	public static Stream<RstCsm> create(String gocId) {
		List<RstCsm> rstList = new ArrayList<RstCsm>();
		Map<ELossStep, Double> lossRatioMap = new HashMap<ELossStep, Double>();

		List<MstRollFwd> rollFwdList = PrvdMst.getMstRollFwdList().stream().filter(s -> s.getCsmAplyYn().isTrueFalse())
				.collect(toList());
		Map<ERollFwdType, ERollFwdType> priorCloseMap = PrvdMst.getPriorCloseStepMap(rollFwdList);

		Map<ELossStep, Double> fulfillMap = RstDao.getRstEpvLossStep(bssd, gocId).stream()
				.collect(toMap(RstLossStep::getLossStep, RstLossStep::getFulfillAmt, (s, u) -> s + u));

		Map<String, List<RstBoxGoc>> rstBoxMap = BoxDao.getRstBoxGoc(bssd, gocId)
				.collect(groupingBy(RstBoxGoc::getCalcId, toList()));

		RstCsm priorClose = new RstCsm();
		RstCsm initClose = new RstCsm();

		for (MstRollFwd aa : rollFwdList) {
			ERollFwdType currRollFwd = aa.getRollFwdType();
			ERollFwdType prevRollFwd = priorCloseMap.get(currRollFwd);

			double fulfillAmt = aa.getLossStep() == null ? 0.0 : fulfillMap.getOrDefault(aa.getLossStep(), 0.0);

			for (MapJournalRollFwd journal : getJournalList(currRollFwd)) {
				MstCalc mstCalc = journal.getMstCalc();

				double lossRatio = lossRatioMap.getOrDefault(mstCalc.getLossStep(), 0.0);

				switch (mstCalc.getCalcType()) {
				case BOX:
				case WATERFALL:
				case REF:
					rstList.addAll(createFromBox(gocId, journal,
							rstBoxMap.getOrDefault(journal.getAppliedCalcId(), new ArrayList<RstBoxGoc>()), lossRatio));
					break;

				case NATIVE:
					rstList.add(createFromNative(gocId, journal, mstCalc, priorClose, initClose));
					break;

				case DELTA_SUM:
					RstCsm tempRst = createCloseStep(gocId, aa, journal.getMstCalc(), rstList, prevRollFwd, fulfillAmt);
					rstList.add(tempRst);
					priorClose = tempRst;
					break;

				default:
					break;
				}
			}

			if (getJournalList(currRollFwd).size() == 0 && currRollFwd.isClose()) {
				MstCalc mstDelaSum = PrvdMst.getMstCalcDeltaSum();
				RstCsm tempRst = createCloseStep(gocId, aa, mstDelaSum, rstList, prevRollFwd, fulfillAmt);
				priorClose = tempRst;

				// Loss Ratio put to Map!!!!
				if (aa.getLossStep() != null) {
					lossRatioMap.put(aa.getLossStep(), tempRst.getLossRatio());
				}
				// Init Close Set!!!
				if (aa.getRollFwdType().equals(ERollFwdType.INIT_CLOSE)) {
					initClose = tempRst;
				}

				rstList.add(tempRst);
			}
		}

		return rstList.stream();
	}
```
{% endtab %}
{% endtabs %}
