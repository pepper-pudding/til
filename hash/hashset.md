# HashSet

{% embed url="https://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html" %}

HashSet은 Set 인터페이스의 구현 클래스입니다. 그렇기에 Set의 성질을 그대로 상속받습니다. Set은 객체를 중복해서 저장할 수 없고 하나의 null 값만 저장할 수 있습니다. 또한 저장 순서가 유지되지 않습니다. 만약 요소의 저장 순서를 유지해야 한다면 JDK 1.4부터 제공하는 LinkedHashSet 클래스를 사용하면 됩니다.&#x20;

{% content-ref url="../linked/linkedhashset.md" %}
[linkedhashset.md](../linked/linkedhashset.md)
{% endcontent-ref %}



## HashSet의 구현

* HashSet = HashMap (key = data, value = dummy static object)

HashSet은 HashMap 객체 참조 변수를 가집니다.&#x20;

```java
private transient HashMap<E, Object> map;

// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
```



HashSet 생성자를 보면, 새로운 HashMap 객체를 만들어서 참조 변수에 넣는 것을 볼 수 있습니다.

```java
/**
 * Constructs a new, empty set; the backing HashMap
 * default initial capacity (16) and load factor (0.75).
 */
public HashSet() {
    map = new HashMap<>();
}
```



Map은 key와 value 모두 필요합니다. 그런데 HashSet은 외부에서 key에 해당하는 데이터만 받습니다. 따라서 미리 만들어둔 Object인 PRESENT를 map.add()할 때 value로 넣어줍니다.

```java
 /**
  * Adds the specified element to this set if it is not already present.
  * More formally, adds the specified element e to this set if
  * this set contains no element e2 such that e.equals(e2).
  * If this set already contains the element, the call leaves the set
  * unchanged and returns false.
  */
 public boolean add(E e) {
     return map.put(e, PRESENT)==null;
 }
```

&#x20;

참고로, value 값으로 넣어주는 Dummy Object인 PRESENT는,  HashSet 내부의 HashMap에서도 공유하고, 다른 HashSet끼리도 공유합니다.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>HashSet 내부의 HashMap에서 공유</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>HashSet끼리 공유</p></figcaption></figure>

##

## HashSet의 특징

Set 인터페이스를 구현한 클래스로는 HashSet과 TreeSet이 있는데 **HashSet의 경우 정렬을 해주지 않고** TreeSet의 경우 자동 정렬을 해준다는 차이점이 있습니다.&#x20;

Set의 가장 큰 장점은 **중복을 자동으로 제거**해준다는 점입니다.



### 중복을 제거하는 방법

HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode() 메소드를 호출해서 해시 코드를 얻어낸 다음 저장되어 있는 객체들의 해시 코드와 비교합니다. 같은 해시 코드가 있다면 다시 equals() 메소드로 두 객체를 비교해서 true가 나오면 동일한 객체로 판단하고 중복 저장을 하지 않습니다.

문자열을 HashSet에 저장할 경우, 같은 문자열을 갖는 String객체는 동일한 객체로 간주되고 다른 문자열을 갖는 String객체는 다른 객체로 간주됩니다. 그 이유는 String클래스가 hashCode()와 equals() 메소드를 재정의해서 같은 문자열일 경우 hashCode()의 리턴 값을 같게, equals()의 리턴 값은 true가 나오도록 했기 때문입니다.



### 값 조회, 추가, 삭제

HashSet은 해시 함수가 버킷 사이에 요소를 적절하게 분산시킨다고 가정했을 때 기본 작업(add(), remove(), size())에 대해 일정한 시간의 성능을 제공합니다. HashSet을 iterate하려면&#x20;

$$
HashSet 인스턴스의 크기(요소 수) + 백업 HashMap 인스턴스의 용량(버킷 수)
$$

에 비례하는 시간이 필요합니다. 따라서 iterate 성능이 중요한 경우 초기 용량을 너무 높게(또는 부하 계수를 너무 낮게) 설정하지 않는 것이 매우 중요합니다.&#x20;



### Slower than List to add or remove

비선형 구조이기에 순서가 없으며 인덱스도 존재하지 않습니다. 그렇기 때문에 값을 추가하거나 삭제할 때에는 추가 혹은 삭제하고자 하는 값이 Set 내부에 있는지 먼저 검색을 한 다음에 수행해야 하므로 List 구조에 비해 속도가 느립니다.



### Iterator : fail-fast

HashSet의 iterate 메서드에 의해 반환된 iterator들은 fail-fast 방식을 따릅니다.&#x20;

iterator가 생성된 이후에, iterator 자체의 remove() 메서드를 통하지 않고 어떤 방식으로든 HashSet이 수정된 경우 iterator는 [ConcurrentModificationException](https://docs.oracle.com/javase/7/docs/api/java/util/ConcurrentModificationException.html)을 던집니다.

동기화되지 않은 동시 수정이 있는 경우 HashSet은 어떤 확실한 보증도 해줄 수 없기 때문에 최선의 노력으로 ConcurrentModificationException을 발생시킵니다.



### Not Thread-Safe

**HashSet은 동기화되지 않습니다.** 여러 스레드가 HashSet에 동시에 액세스하고 스레드 중 하나 이상이 HashSet을 수정하는 경우 외부에서 동기화해야 합니다_._

일반적으로 [`Collections.synchronizedSet`](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#synchronizedSet\(java.util.Set\)) 메서드를 사용하여 HashSet을 래핑하는 방식을 통해 우발적인 비동기 엑세스를 방지합니다.

```java
   SortedSet s = Collections.synchronizedSet(new HashSet(...));
```



## 참고자료

{% embed url="https://codingdog.tistory.com/entry/java-hashset-%EC%96%B4%EB%96%BB%EA%B2%8C-%EA%B5%AC%ED%98%84%EC%9D%B4-%EB%90%98%EC%96%B4-%EC%9E%88%EC%9D%84%EA%B9%8C%EC%9A%94" %}

