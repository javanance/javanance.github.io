---
description: MdlRstDac.create()
---

# M86 : RST BOX DAC

### FROM

* RST\_BOX
* RST\_DAC (DAC\_PREV)
* RATIO\_DAC (상각률)
* MST\_ROLL\_FWD
* MAP\_JOURNAL\_ROLL\_FWD&#x20;
  * MST\_CALC
  * MAP\_CF\_GROUP **G\_DAC**&#x20;

### TO&#x20;

* RST\_DAC

### DO

* GoC별 미상각 신계약비 잔액의 변동을 산출함
* 전기 잔액 및 신계약 유입으로 인한 증가분&#x20;
* &#x20;DAC관련 경험조정 및 미래 CF 변동 반영&#x20;
* 당기 상각액&#x20;
* 기말 DAC금액&#x20;

### DESC

#### Rollfwd 처리내용&#x20;

* PREV\_CLOSE 직전 결산 : **`NATIVE`**
  * DAC (직접신계약비)는 직전 결산 시 미상각잔액을 기시잔액 기준으로 미상각잔액의 변동을 산출
  * RST\_DAC의 직전 결산 CLOSE 값이 없거나,  update된 경우 전기기말 ≠ 당기 기시 문제가 발생할 수 있음
* INIT\_RECOG 신계약 최초인식:  **`BOX`**
  * 신계약 Add로 인한 미상각잔액 추가 인식분&#x20;
* 경험조정 (대상 : CALC\_ID.CF\_GROUP :_DAC1_) : **`BOX`**
  * (-) CF\_RELEASE 예상 CF RELEASE (해당 기간 서비스 제공)&#x20;
  * (+) CASH\_RELEASE 실제 CF RELEASE
    * DAC1 보험료 유관 신계약비 항목의 경험조정은 CSM 미래서비스관련 변동에 반영되기 때문에 미상각 잔액에 경험조정 효과만큼 반영함 (B120 총수익인식 대상 검증에서 중복처리되지 않도록 하기 위함)  &#x20;
    * DAC2 보험료 무관 신계약비 항목의 경험조정은 당기 수익-비용으로 처리되기 때문에 미상각잔액에 경험조정 효과를 추가로 반영하지 않음 &#x20;
* 미래 서비스 관련 변동 : **`BOX` **&#x20;
  * TIME\_CHANGE 시점 변경&#x20;
  * ACTU\_CHANGE 계리적가정 변경&#x20;
  * FINC\_CHANGE 경제적가정 변경 &#x20;
  * DISC\_CHANGE 재량 변경
* MOVE\_CLOSE : **`DELTA_SUM`**
  * 상각대상 금액을 확정하기 위해 기시\~기중 변동 합산금액을 중간 집계&#x20;
* COVERAGE\_RELEASE 상각:  **`NATIVE`**
  * &#x20;RATIO\_DAC에 정의된 GoC별 상각률 참조 (연관 JOB : M36)
* CURR\_CLOSE 기말 결산 : **`DELTA_SUM`**
  * 상각 후 미상각 잔액&#x20;



#### &#x20;calcType  처리내용&#x20;

* calcType = **`BOX`**, WATERFALL, REF
  * MAP\_JOURNAL\_ROLL\_FWD COA = DAC인 CALC\_ID 식별&#x20;
  * RST\_BOX에서 위의 Calc\_ID별 계산결과 집계&#x20;
  * getCoaValue : MAP\_JOURNAL\_ROLL\_FWD의 분개기준 차대변 항목에 따라 부호 처리&#x20;
* calcType = **`NATIVE` **&#x20;
  * DAC\_PREV : 전기 잔액을 가져오기&#x20;
  * DAC\_RELEASE\_1 : DAC 상각&#x20;
    * GoC별 상각률은 RATIO\_DAC에서 조회
    * &#x20;Default =0.03
    * 상각액이 음수인 경우 0 처리
* calcType = **`DELTA_SUM`**
  * DeltaAmt, CloseAmt&#x20;

### SRC

```java

		List<RstDac> rstList = new ArrayList<RstDac>();

		List<MstRollFwd> rollFwdList = PrvdMst.getMstRollFwdList().stream().filter(s -> s.getDacAplyYn().isTrueFalse())
				.collect(toList());

		Map<ERollFwdType, ERollFwdType> priorCloseMap = PrvdMst.getPriorCloseStepMap(rollFwdList);

		Map<String, List<RstBoxGoc>> rstBoxMap = BoxDao.getRstBoxGoc(bssd, gocId)
				.collect(groupingBy(RstBoxGoc::getCalcId, toList()));

		RstDac priorClose = new RstDac();

		for (MstRollFwd aa : rollFwdList) {
			ERollFwdType currRollFwd = aa.getRollFwdType();
			ERollFwdType prevRollFwd = priorCloseMap.get(currRollFwd);

			for (MapJournalRollFwd journal : getJournalList(currRollFwd)) {
				MstCalc mstCalc = journal.getMstCalc();

				switch (mstCalc.getCalcType()) {
				case BOX:
				case WATERFALL:
				case REF:
					rstList.addAll(createFromBox(gocId, journal,
							rstBoxMap.getOrDefault(journal.getAppliedCalcId(), new ArrayList<RstBoxGoc>())));
					break;

				case NATIVE:
					rstList.add(createFromNative(gocId, journal, mstCalc, priorClose));
					break;

				case DELTA_SUM:
					RstDac tempRst = createCloseStep(gocId, aa, journal.getMstCalc(), rstList, prevRollFwd);
					rstList.add(tempRst);
					priorClose = tempRst;
					break;

				default:
					break;
				}
			}
			if (getJournalList(currRollFwd).size() == 0 && currRollFwd.isClose()) {
				MstCalc mstDeltaSum = PrvdMst.getMstCalcDeltaSum();
				RstDac tempRst = createCloseStep(gocId, aa, mstDeltaSum, rstList, prevRollFwd);
				priorClose = tempRst;
				rstList.add(tempRst);
			}
		}
		return rstList.stream();

	
```
