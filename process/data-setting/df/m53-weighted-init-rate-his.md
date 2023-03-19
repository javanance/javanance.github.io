# M53 : Weighted init rate his

### FROM&#x20;

* &#x20;대상 : RST\_EPV\_NGOC (신계약이 유입된 GoC list)
  * 기준일자가 현재 결산일(bssd)과 같거나 이전인 경우&#x20;
* DF\_LV1\_CURR\_RATE

### TO&#x20;

* DF\_LV2\_WGHT\_HIS

### DO

* getInitCurveYymm
* 가중치 : getOutEpvAmt

### DESC

* GoC별 최초인식년월을 처음인식한 날짜의 커브를 가져왔을 경우에 이게 무슨 의미인지 생각해봐야 함 !!! -> 확인 후 문서 재작성&#x20;
* 처음에 가중평균 최초할인율을 구성할 때는 매월 신계약이 유입되는 경우를 고려할때 매달 다른 최초할인율을 적용하는 것을 고려하였으나.
* 현재는 GoC의 최초일자를 mst goc의 최초인식년월을 사용하면 동일한 최초인식년월으로 계산됨.&#x20;

### SRC
