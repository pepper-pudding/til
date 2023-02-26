# TreeMap

{% embed url="https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html" %}

TreeMap은 이진 트리를 기반으로 한 Map 컬렉션입니다.&#x20;

같은 Tree구조로 이루어진 TreeSet과의 차이점은 TreeSet은 그냥 값만 저장한다면 TreeMap은 키와 값이 저장됩니다. 즉, Entry를 저장합니다.

TreeMap은 일반적으로 Map으로써의 성능이 HashMap보다 떨어집니다. TreeMap은 데이터를 저장할 때 즉시 정렬하기에 추가나 삭제가 HashMap보다 오래 걸립니다. 하지만 정렬된 상태로 Map을 유지해야 하거나 정렬된 데이터를 조회해야 하는 범위 검색이 필요한 경우 TreeMap을 사용하는 것이 효율성 면에서 좋습니다.



## TreeMap의 특징

### Sort

TreeMap에 객체를 저장하면 **자동으로 정렬**되는데, 키는 저장과 동시에 자동 오름차순으로 정렬되고 숫자 타입일 경우에는 값으로, 문자열 타입일 경우에는 유니코드로 정렬합니다. 정렬 순서는 기본적으로 부모 키값과 비교해서 키 값이 낮은 것은 왼쪽 자식 노드에 키값이 높은 것은 오른쪽 자식 노드에 Map.Entry 객체를 저장합니다.&#x20;



### Red-Black Tree

TreeMap은 이진 탐색 트리의 문제점을 보완한 레드-블랙 트리(Red-Black Tree)로 이루어져 있습니다.&#x20;

일반적인 이진 탐색 트리는 트리의 높이만큼 시간이 필요합니다. 값이 전체 트리에 잘 분산되어 있다면 효율성에 있어 크게 문제가 없으나 데이터가 들어올 때 값이 한쪽으로 편향되게 들어올 경우 한쪽 방면으로 크게 치우쳐진 트리가 되어 굉장히 비효율적인 퍼포먼스를 냅니다.&#x20;

이 문제를 보완하기 위해 레드 블랙 트리가 등장하였습니다. 레드 블랙 트리는 부모 노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여 데이터의 추가나 삭제 시 트리가 한쪽으로 치우쳐지지 않도록 균형을 맞추어줍니다.



### Not Thread-Safe

**TreeMap은 동기화되지 않습니다.** 여러 스레드가 TreeMap에 동시에 액세스하고 스레드 중 하나 이상이 TreeMap을 수정하는 경우 외부에서 동기화해야 합니다_._

일반적으로 [Collections.synchronizedSortedMap](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#synchronizedSortedMap\(java.util.SortedMap\)) 메서드를 사용하여 TreeMap을 래핑하는 방식을 통해 우발적인 비동기 엑세스를 방지합니다.

```java
   SortedMap map = Collections.synchronizedSortedMap(new TreeMap(...));
```



## 참고자료

{% embed url="https://coding-factory.tistory.com/557" %}
