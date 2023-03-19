---
description: MdlRstBox.createGoc()
---

# M75 : RST BOX GROUP

### FROM

* RST\_BOX\_DETAIL&#x20;

### TO&#x20;

* RST\_BOX

### DESC

* RST\_BOX\_DETAIL 을 GROUPING 하여 RST\_BOX에 적재함.&#x20;
* GROUP BY  :baseYymm, gocId, liabType, mstRunset, calcId, stStatus, endStatus, newContYn

### SRC

{% tabs %}
{% tab title="createGoc()" %}
{% code title="MdlRstBox.java" %}
```java
	public static Stream<RstBoxGoc> createGoc() {
		List<String> gocIdList = PrvdMst.getGocIdList();
		return BoxDao.getRstBoxGroupBy(bssd).stream().filter(s -> gocIdList.contains(s.getGocId()));
	}
```
{% endcode %}
{% endtab %}

{% tab title="getRstBoxGroupBy()" %}
{% code title="BoxDao.java" %}
```java
public static List<RstBoxGoc> getRstBoxGroupBy(String bssd ){
String query = "select new com.gof.entity.RstBoxGoc(a.baseYymm, a.gocId, a.liabType, a.mstRunset, a.calcId, a.stStatus, a.endStatus, a.newContYn"
		+ ", sum(a.cfAmt), sum(a.prevCfAmt), sum(a.deltaCfAmt), sum(a.boxValue)) "
		+ "from RstBoxDetail a "
		+ "where a.baseYymm  = :bssd "
		+ "group by a.baseYymm, a.gocId, a.liabType, a.mstRunset, a.calcId, a.stStatus, a.endStatus, a.newContYn"
		;

Query<RstBoxGoc> q = session.createQuery(query, RstBoxGoc.class);
q.setParameter("bssd", bssd);
return q.getResultList();
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
