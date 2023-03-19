---
description: MdlRstBox.createDetail()
---

# M71 : RST BOX DETAIL

### FROM

* CF\_LV4\_DF
* MAP\_RUNSET\_CALC
  * MST\_RUNSET
  * MST\_CALC
  * MAP\_CF\_GROUP

### TO

* RST\_BOX\_DETAIL

### DO

* Data setting에서 만들어진 CF\_LV4\_DF 를 이용하여 주어진 RunsetId별 calcId 항목을 계산하는 작업을 수행함. 산출이 대상이 되는 CF 및 그에 따른 필요 DF (할인계수)는 CF\_LV4\_DF에서 참조
* Runset별 계산해야 하는 항목(calcId)에 대한 정의는 MAP\_RUNSET\_CALC에 정의
* 계산항목의 대상 및 사용 할인계수, 부호처리, serviceTiming,  사용 LossRatio는 MST\_CALC에 정의

### DESC

* &#x20;MAP\_RUNSET\_CALC에서 CalcType = BOX인 대상인 Runset만 추출
* CF\_LV4\_DF에서 deltaCfAmt가 0이 아닌 값만 가져옴
* CF\_LV4\_DF의 현금흐름 중 MAP\_RUNSET\_CALC  check : 대상
  * LiabType : LRC, LIC
  * calcId에 해당하는 CF GROUP에 속하는 CF TYPE &#x20;
  * ServiceTiming :START , MID, END
  * runsetId
* BoxValue는 EBoxModel에서 처리 (ENUM의 Box Value 참조) => link걸기 !!!!



### SRC

{% tabs %}
{% tab title="createDetail()" %}
{% code title="MdlRstBox.java" %}
```java
	public static Stream<RstBoxDetail> createDetail() {
		return createDetail(null);
	}
	
	public static Stream<RstBoxDetail> createDetail(String gocId) {
		List<MapRunsetCalc> runsetList = getRunsetCalcList(ECalcType.BOX);

		return CfDao.getCfLv4DfStream(bssd, gocId).filter(s -> s.getDeltaCfAmt() != 0.0)
				.map(s -> FacBoxDetail.build(bssd, s, runsetList)).flatMap(s -> s.stream())
		;
	}
```
{% endcode %}
{% endtab %}

{% tab title="build(bssd, s, runsetList)" %}
{% code title="FacBoxDetail.java" %}
```java
	public static List<RstBoxDetail> build(String bssd, CfLv4Df cf, List<MapRunsetCalc> calcList) {
		List<RstBoxDetail> rstList = new ArrayList<RstBoxDetail>();

		for (MapRunsetCalc mapCalc : calcList) {
			if (cf.check(mapCalc)) {
				rstList.add(build(bssd, cf, mapCalc));
			}
		}
		return rstList;
	}
```
{% endcode %}
{% endtab %}

{% tab title="check()" %}
{% code title="CfLv4Df.java" %}
```java
	public boolean check(MapRunsetCalc mapCalc) {
		MstCalc calc = mapCalc.getMstCalc();
		return liabType.equals(calc.getLiabType())
					  && calc.getCfGroup().contatins((cfType))
						&& calc.getServiceTiming().isApplied(this)
						&& runsetId.equals(mapCalc.getRunsetId())
							;
	}
```
{% endcode %}
{% endtab %}

{% tab title="build(bssd, cf, mapCalc)" %}
{% code title="FacBoxDetail.java" %}
```java
	private static RstBoxDetail build(String bssd, CfLv4Df cf, MapRunsetCalc calc) {

		double cfAmt = calc.getMstCalc().getBoxId().equals(EBoxModel.ZU) && cf.isFutureCf(0.0) ? 0.0 : cf.getCfAmt();
		double prevCfAmt = calc.getMstCalc().getBoxId().equals(EBoxModel.ZU) && cf.isFutureCf(0.0) ? 0.0
				: cf.getPrevCfAmt();
		double deltaCfAmt = cfAmt - prevCfAmt;
		// boxValue 생성!!
		double boxValue = calc.getMstCalc().getSignAdjust().getAdj() * cf.getBoxValue(calc.getMstCalc().getBoxId());
		
		return RstBoxDetail.builder().baseYymm(bssd).gocId(cf.getGocId()).liabType(cf.getLiabType())
				.mstRunset(calc.getMstRunset()).calcId(calc.getMstCalc().getCalcId()).deltaGroup(cf.getDeltaGroup())
				.stStatus(cf.getStStatus()).endStatus(cf.getEndStatus()).newContYn(cf.getNewContYn())
				.cfKeyId(cf.getCfKeyId()).cfType(cf.getCfType()).cfTiming(cf.getCfTiming()).outflowYn(cf.getOutflowYn())
				.cfMonthNum(cf.getCfMonthNum()).setlAftPassMmcnt(cf.getSetlAftPassMmcnt())
				.boxId(calc.getMstCalc().getBoxId()).slidingNum(cf.getSlidingNum()).cfAmt(cfAmt).prevCfAmt(prevCfAmt)
				.deltaCfAmt(deltaCfAmt).boxValue(boxValue).appliedRate(cf.getCurrRate()) // TODO!!!!!
				.remark("").lastModifiedBy(GmvConstant.getLastModifier()).lastModifiedDate(LocalDateTime.now()).build();
	}
```
{% endcode %}
{% endtab %}

{% tab title="getBoxValue()" %}
{% code title="CfLv4Df.java" %}
```java
	public double getBoxValue(EBoxModel box) {
		return getAppiedCfAmt() * getBoxValueMap().getOrDefault(box, 0.0);
	}
	// getAppiedCfAmt() = deltaCfAmt
	
	@Override
	public Map<EBoxModel, Double> getBoxValueMap() {
		if(boxValueMap.isEmpty()) {
			boxValueMap = EBoxModel.getBoxValueMap(this, "AddOn");
		}
		if(!isFutureCf(0.0) && !boxValueMap.containsKey(EBoxModel.ZU)) {
			boxValueMap.put(EBoxModel.ZU, -1.0);
		}
		return boxValueMap;
	}
```
{% endcode %}
{% endtab %}

{% tab title="getBoxValueMap()" %}
{% code title="EBoxModel.java" %}
```java
public static Map<EBoxModel, Double> getBoxValueMap(Compoundable rates, String type){
EnumMap<EBoxModel, Double> rstMap = new EnumMap<>(EBoxModel.class);
for(EBoxModel aa : EBoxModel.values()){
	if(aa.isContainedIn(type)){
		rstMap.put(aa, aa.getValue(rates));
	}
}
return rstMap;
}

public double getValue(Compoundable rates){
return toDf.getValue(rates) - fromDf.getValue(rates);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
