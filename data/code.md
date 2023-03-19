---
description: Enums
---

# Code : ENUMs

테이블에서 설정 가능한 코드값은 프로그램 내에서 Enum의 형태로 정의하여 관리됨.

IFRS 17관련 CODE 목록 및 Code는 아래와 같음.

{% hint style="info" %}
계산 관련

* 날짜계산 : 기간, 기간단위 , ECompound (기간, 방식),EDayCount일수계산방식 , ,time bucket
* 연산 : EOperator (부호처리)
* EBoolean
{% endhint %}

{% hint style="info" %}
BOX Model

* EDiscFactor : 필요할인계수&#x20;
* EBoxModel : 할인계수들의 차이&#x20;
* CF : Cf timing&#x20;
* EServiceTiming (미래 서비스 관련 , 현재 or 과거 서비스 관련 , ALL 구분없음 )
* EValueDeco 부호처리&#x20;
{% endhint %}

{% hint style="info" %}
회계 요건관련  : 구분의 이유 : 왜 필요한가 ?

* ELiabType 부채구분
* EConvType 전환방식 (완전소급, 수정소급, 공정가치법 )
* EContStatus 계약상태구분&#x20;
* EServiceTiming (미래 서비스 관련 , 현재 or 과거 서비스 관련 , ALL 구분없음 )
* ECfType : 회계처리용 cf 구분&#x20;
* ELossStep : 스텝별 손실요소 배분&#x20;
* EProfitDiv GoC 수익성구분&#x20;
* ECohortYear / ECohortQurt
* EGocStatus : 스텝별 손익 상태 : 손실요소 환입(전입) 처리용&#x20;
* ECalcType
* ECalcMethod
* ERollFwdType
* ERsDivType
* ECoa&#x20;
{% endhint %}

* 대상 목록



* [ ] TODO : 주제별로 다룰 때 사용되는 코드 값 link 걸어두기
* [ ] 코드 별 유효코드값 및 코드 값 별 처리사항  상세하게 정의&#x20;

&#x20;
