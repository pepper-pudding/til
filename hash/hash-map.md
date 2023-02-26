---
description: 수행 시간에 몰빵!
---

# Hash Map

## HashMap

{% embed url="https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html" %}

HashMap은 Map 인터페이스를 구현한 대표적인 Map Collection입니다.

Map 인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있습니다. Map은 키와 값으로 구성된 Entry 객체를 저장하는 구조를 가지고 있는 자료구조입니다. 여기서 키와 값은 모두 객체입니다. 값은 중복 저장될 수 있지만 키는 중복 저장될 수 없습니다. 만약 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대치됩니다.&#x20;

HashMap은 해싱을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보입니다.



## HashMap의 구현

{% content-ref url="hashmap.md" %}
[hashmap.md](hashmap.md)
{% endcontent-ref %}



## 해시 분포와 해시 충돌

HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하는데, 결과 자료형은 int입니다. 32비트 정수 자료형으로는 완전한 해시 함수를 만들 수 없습니다.&#x20;

논리적으로 생성 가능한 객체의 수가 2^32보다 많을 수 있기 때문이며, 모든 HashMap 객체에서 O(1)을 보장하기 위해 랜덤 접근이 가능하게 하려면 원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문입니다.

따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 associative array 구현체에서는 메모리를 절약하기 위하여 실제 해시 함수의 표현 정수 범위보다, M개의 원소가 있는 배열만을 사용합니다. 따라서 다음과 같이 객체에 대한 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용합니다.

```java
int index = X.hashCode() % M;
```

이 코드와 같은 방식을 사용하면, 서로 다른 해시 코드를 가지는 서로 다른 객체가 1/M의 확률로 같은 해시 버킷을 사용하게 됩니다. 이는 해시 함수가 얼마나 해시 충돌을 회피하도록 잘 구현되었느냐에 상관없이 발생할 수 있는 또 다른 종류의 해시 충돌입니다.

이렇게 해시 충돌이 발생하더라도 키-값 쌍 데이터를 잘 저장하고 조회할 수 있게 하는 방식에는 대표적으로 두 가지가 있는데, 하나는 Open Addressing이고, 다른 하나는 Separate Chaining입니다. 이 둘 외에도 해시 충돌을 해결하기 위한 다양한 자료 구조가 있지만, 거의 모두 이 둘을 응용한 것이라고 할 수 있습니다.



## Java HashMap에서 사용하는 해시 충돌 처리 방식?

Java HashMap에서 사용하는 방식은 **Separate Channing**입니다.&#x20;

1. Open Addressing은 데이터를 삭제할 때 처리가 효율적이기 어려운데, HashMap에서 remove() 메서드는 매우 빈번하게 호출될 수 있기 때문입니다.
2. 게다가 HashMap에 저장된 키-값 쌍 개수가 일정 개수 이상으로 많아지면, 일반적으로 Open Addressing은 Separate Chaining보다 느립니다. Open Addressing의 경우 해시 버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아지기 때문입니다. 반면 Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 '조정'할 수 있다면 Worst Case 또는 Worst Case에 가까운 일이 발생하는 것을 줄일 수 있습니다.(여기에 대해서는 `보조 해시 함수`에서 설명하겠습니다)



### Java 7 HashMap의 put()

Seperate Chaining 방식을 사용하고 있음을 알 수 있습니다.

```java
public V put(K key, V value) { if (table == EMPTY_TABLE) { inflateTable(threshold); // table 배열 생성 } // HashMap에서는 null을 키로 사용할 수 있다. if (key == null) return putForNullKey(value); // value.hashCode() 메서드를 사용하는 것이 아니라, 보조 해시 함수를 이용하여 // 변형된 해시 함수를 사용한다. "보조 해시 함수" 단락에서 설명한다.  
    int hash = hash(key);

    // i 값이 해시 버킷의 인덱스이다.
    // indexFor() 메서드는 hash % table.length와 같은 의도의 메서드다.
    int i = indexFor(hash, table.length);



    // 해시 버킷에 있는 링크드 리스트를 순회한다.
    // 만약 같은 키가 이미 저장되어 있다면 교체한다.
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }

    // 삽입, 삭제 등으로 이 HashMap 객체가 몇 번이나 변경(modification)되었는지
    // 관리하기 위한 코드다.
    // ConcurrentModificationException를 발생시켜야 하는지 판단할 때 사용한다.
    modCount++;


    // 아직 해당 키-값 쌍 데이터가 삽입된 적이 없다면 새로 Entry를 생성한다. 
    addEntry(hash, key, value, i);
    return null;
}
```



### Java 8 HashMap의 put()

Java 8에서는 Java 7에서 볼 수 있는 것보다 더 발전된 방식의 HashMap을 사용합니다.

**데이터의 개수가 많아지면, Separate Chaining에서 LinkedList 대신 Tree를 사용**하기 때문입니다.



Java 2부터 Java 7까지의 HashMap에서 Separate Chaining 구현 코드는 조금씩 다르지만, 구현 알고리즘 자체는 같았습니다. 만약 객체의 해시 함수 값이 균등 분포(uniform distribution) 상태라고 할 때, get() 메서드 호출에 대한 기댓값은

$$
E\left(\frac{N}{M}\right)
$$

입니다. 그러나 Java 8에서는 이보다 더 나은

$$
E\left(log\frac{N}{M}\right)
$$

을 보장합니다. 데이터의 개수가 많아지면, Separate Chaining에서 LinkedList 대신 Tree를 사용하기 때문입니다.



#### 일정 개수 이상일 때 LinkedList 보다 Tree를 사용하는 것이 더 좋은 이유?

데이터의 개수가 많아지면&#x20;

$$
\left(\frac{N}{M}\right)
$$

와

$$
\left(log\frac{N}{M}\right)
$$

의 차이는 무시할 수 없습니다. 게다가 **실제 해시 값은 균등 분포가 아닐** 뿐더러, 설사 균등 분포를 따른다고 하더라도 birthday problem이 설명하듯 **일부 해시 버킷 몇 개에 데이터가 집중될 수 있습니다**. 그래서 데이터의 개수가 일정 이상일 때에는 LinkedList 대신 Tree를 사용하는 것이 성능상 이점이 있습니다.



```java
static final int TREEIFY_THRESHOLD = 8;

static final int UNTREEIFY_THRESHOLD = 6;
```

LinkedList를 사용할 것인가 Tree를 사용할 것인가에 대한 기준은 **하나의 해시 버킷에 할당된 키-값 쌍의 개수**입니다. Java 8 HashMap에서는 상수 형태로 기준을 정하고 있습니다. 즉 하나의 해시 버킷에 8개의 키-값 쌍이 모이면 LinkedList를 Tree로 변경합니다. 만약 해당 버킷에 있는 데이터를 삭제하여 개수가 6개에 이르면 다시 LinkedList로 변경합니다. Tree는 LinkedList보다 메모리 사용량이 많고, 데이터의 개수가 적을 때 트리와 LinkedList의 Worst Case 수행 시간 차이 비교는 의미가 없기 때문입니다. 8과 6으로 2 이상의 차이를 둔 것은, 만약 차이가 1이라면 어떤 한 키-값 쌍이 반복되어 삽입/삭제되는 경우 불필요하게 LinkedList와 Tree를 변경하는 일이 반복되어 성능 저하가 발생할 수 있기 때문입니다.



Java 8 HashMap에서는 Entry 클래스 대신 Node 클래스를 사용합니다. Node 클래스 자체는 사실상 Java 7의 Entry 클래스와 내용이 같지만, LinkedList 대신 Tree를 사용할 수 있도록 하위 클래스인 TreeNode가 있다는 것이 Java 7 HashMap과 다릅니다.

이때 사용하는 트리는 **Red-Black Tree**인데, Java Collections Framework의 TreeMap과 구현이 거의 같습니다. **트리 순회 시 사용하는 대소 판단 기준은 해시 함수 값**입니다. 해시 값을 대소 판단 기준으로 사용하면 **Total Ordering에 문제**가 생기는데, Java 8 HashMap에서는 이를 **tieBreakOrder() 메서드로 해결**합니다.



## 해시 버킷 동적 확장 <a href="#undefined" id="undefined"></a>

해시 버킷의 개수가 적다면 메모리 사용을 아낄 수 있지만 해시 충돌로 인해 성능 손실이 발생합니다. 그래서 HashMap은 키-값 쌍 데이터 개수가 일정 개수 이상이 되면, **해시 버킷의 개수를 두 배로 늘립니다**. 이렇게 해시 버킷 개수를 늘리면

$$
\left(\frac{N}{M}\right)
$$

값도 작아져, 해시 충돌로 인한 성능 손실 문제를 어느 정도 해결할 수 있습니다.

해시 버킷 개수의 기본값은 16이고, 데이터의 개수가 임계점에 이를 때마다 해시 버킷 개수의 크기를 두 배씩 증가시킵니다. 버킷의 최대 개수는 2^30개입니다. 그런데 이렇게 버킷 개수가 두 배로 증가할 때마다, 모든 키-값 데이터를 읽어 새로운 Separate Chaining을 구성해야 하는 문제가 있습니다.&#x20;

HashMap 생성자의 인자로 초기 해시 버킷 개수를 지정할 수 있으므로, 해당 HashMap 객체에 저장될 데이터의 개수가 어느 정도인지 예측 가능한 경우에는 이를 생성자의 인자로 지정하면 불필요하게 Separate Chaining을 재구성하지 않게 할 수 있습니다.

<pre class="language-java"><code class="lang-java">// 인자로 사용하는 newCapacity는 언제나 2a이다.
void resize(int newCapacity) {  
    Entry[] oldTable = table;
    int oldCapacity = oldTable.length;

    // MAXIMIM_CAPACITY는 2^30이다.
    if (oldCapacity == MAXIMUM_CAPACITY) {
        threshold = Integer.MAX_VALUE;
        return;
    }

    Entry[] newTable = new Entry[newCapacity];


    // 새 해시 버킷을 생성한 다음 기존의 모든 키-값 데이터들을
    // 새 해시 버킷에 저장한다.
    <a data-footnote-ref href="#user-content-fn-1">transfer</a>(newTable, initHashSeedAsNeeded(newCapacity));
    table = newTable;
    threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
}
</code></pre>

```java
void transfer(Entry[] newTable, boolean rehash) {
    int newCapacity = newTable.length;
    // 모든 해시 버킷을 순회하면서
    for (Entry<K,V> e : table) {
        // 각 해시 버킷에 있는 링크드 리스트를 순회하면서
        while(null != e) {
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            // 해시 버킷 개수가 변경되었기 때문에
            // index 값(hashCode % M)을 다시 계산해야 한다. 
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        }
    }
}
```

해시 버킷 크기를 두 배로 확장하는 임계점은 현재의 데이터 개수가 **'load factor \* 현재의 해시 버킷 개수'**에 이를 때입니다.



## 보조 해시 함수





## HashMap의 특징

### Not Thread-Safe

**HashMap은 동기화되지 않습니다.** 여러 스레드가 HashMap에 동시에 액세스하고 스레드 중 하나 이상이 HashMap을 수정하는 경우 외부에서 동기화해야 합니다_._

일반적으로 [Collections.synchronizedMap](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html#synchronizedMap\(java.util.Map\)) 메서드를 사용하여 HashMap을 래핑하는 방식을 통해 우발적인 비동기 엑세스를 방지합니다.

```java
   Map map = Collections.synchronizedMap(new HashMap(...));
```

## 참고자료

{% embed url="https://d2.naver.com/helloworld/831311" %}

[^1]: 
