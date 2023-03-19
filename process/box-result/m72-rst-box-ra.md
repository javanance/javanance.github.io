---
description: MdlRstBox.createRa()
---

# M72 : RST BOX RA

### FROM

* MAP\_RUNSET\_CALC
  * MST\_RUNSET
  * MST\_CALC
  * MAP\_CF\_GROUP
* RA\_LV2\_DELTA

### TO&#x20;

* RST\_BOX (RstBoxGoc)

### DESC

* MAP\_RUNSET\_CALC 에서 RA Runset 중&#x20;
  * calcType ='WATERFALL' : 직전결과와의 잔차로 가치 변동을 산출하는 케이스&#x20;
* RA\_LV2\_DELTA 에서 DeltaRaAmt 0이 아닌 경우 집계&#x20;

### SRC

```java
	public static Stream<RstBoxGoc> createRa(String gocId) {
		Map<String, List<MapRunsetCalc>> runsetMap = getRunsetCalcList(ECalcType.WATERFALL).stream()
				.collect(groupingBy(MapRunsetCalc::getRunsetId, toList()));

		return RaDao.getRaLv2Delta(bssd, gocId).stream().filter(s -> s.getDeltaRaAmt() != 0.0)
				.map(s -> FacBoxGoc.build(bssd, s,
						runsetMap.getOrDefault(s.getRunsetId(), new ArrayList<MapRunsetCalc>())))
				.flatMap(s -> s.stream());
	}
```
