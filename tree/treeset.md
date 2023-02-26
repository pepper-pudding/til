# TreeSet

{% embed url="https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html" %}

Java의 TreeSet은 Set 인터페이스(보다 구체적으로는 SortedSet)를 구현합니다. Java에서 TreeSet을 구현하는 TreeSet 클래스는 NavigableSet 인터페이스를 구현하고 AbstractSet 클래스도 상속합니다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>



TreeSet은 데이터 저장을 위해 내부적으로 TreeMap을 사용합니다.&#x20;

기본적으로 TreeSet의 개체 또는 요소는 자연 순서에 따라 오름차순으로 저장됩니다.



## TreeSet의 구현

```
public TreeSet() {
    this(new TreeMap());
}
```

TreeSet의 생성자를 보면 TreeMap 개체를 호출하고 있음을 알 수 있습니다. TreeSet도 HashSet처럼 TreeMap을 기반으로 구현되었으며, key에 데이터를 저장합니다.



## TreeSet 특징

### Iterator : fail-fast

TreeSet의 iterate 메서드에 의해 반환된 iterator들은 fail-fast 방식을 따릅니다.&#x20;

iterator가 생성된 이후에, iterator 자체의 remove() 메서드를 통하지 않고 어떤 방식으로든 HashSet이 수정된 경우 iterator는 [ConcurrentModificationException](https://docs.oracle.com/javase/7/docs/api/java/util/ConcurrentModificationException.html)을 던집니다.

동기화되지 않은 동시 수정이 있는 경우 TreeSet은 어떤 확실한 보증도 해줄 수 없기 때문에 최선의 노력으로 ConcurrentModificationException을 발생시킵니다.



### Not Thread-Safe

**HashSet은 동기화되지 않습니다.** 여러 스레드가 TreeSet에 동시에 액세스하고 스레드 중 하나 이상이 TreeSet을 수정하는 경우 외부에서 동기화해야 합니다_._

일반적으로 [Collections.synchronizedSortedSet](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#synchronizedSortedSet\(java.util.SortedSet\)) 메서드를 사용하여 TreeSet을 래핑하는 방식을 통해 우발적인 비동기 엑세스를 방지합니다.

```java
   Set s = Collections.synchronizedSortedSet(new TreeSet(...));
```



## 참고자료

{% embed url="https://ko.myservername.com/treeset-java-tutorial-with-programming-examples" %}
