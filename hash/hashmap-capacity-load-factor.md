# HashMap의 Capacity와 Load factor

HashMap의 인스턴스에는 성능에 영향을 미치는 두 가지 매개변수가 있다.

1. capacity : 해시 버킷 수
2. load factor : 해시 버킷이 얼마나 가득 찼는지 보여주는 수치

해시 버킷 크기를 두 배로 확장하는 임계점은 현재의 데이터 개수가 **'load factor \* 현재의 해시 버킷 개수'**에 이를 때입니다.



~~_용량 은 해시 테이블 의 버킷 수이고 초기 용량은 단순히 해시 테이블이 생성된 시점의 용량입니다. 로드 팩터 는 용량이 자동으로 증가하기 전에 해시 테이블이 얼마나 가득 찰 수 있는지를 측정한 것입니다 . 해시 테이블의 항목 수가 로드 팩터와 현재 용량의 곱을 초과하면 해시 테이블이 버킷 수의 약 2배가 되도록 해시 테이블이 다시 해시됩니다(즉, 내부 데이터 구조가 다시 작성됨)._~~

~~__~~\
~~_일반적으로 기본 부하 계수(.75)는 시간과 공간 비용 간에 적절한 절충안을 제공합니다. 값이 높을수록 공간 오버헤드는 줄어들지만 조회 비용은 증가합니다( get 및 put 을 포함하여 HashMap 클래스 의 대부분의 작업에 반영됨 ). 재해시 작업의 수를 최소화하기 위해 초기 용량을 설정할 때 맵의 예상 항목 수와 로드 요소를 고려해야 합니다. 초기 용량이 로드 팩터로 나눈 최대 항목 수보다 큰 경우 재해시 작업이 발생하지 않습니다._~~

~~__~~

~~_HashMap 인스턴스 에 많은 매핑을 저장 해야 하는 경우 충분히 큰 용량으로 생성하면 테이블을 확장하기 위해 필요에 따라 자동 재해싱을 수행하는 것보다 매핑을 더 효율적으로 저장할 수 있습니다. 동일한 키를 여러 개 사용 `hashCode()`하는 것은 모든 해시 테이블의 성능을 저하시키는 확실한 방법입니다. 영향을 개선하기 위해 키가_~~ [~~_`Comparable`_~~](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)~~_인 경우 이 클래스는 키 간의 비교 순서를 사용하여 관계를 끊을 수 있습니다._~~



{% embed url="https://wjdtn7823.tistory.com/75" %}