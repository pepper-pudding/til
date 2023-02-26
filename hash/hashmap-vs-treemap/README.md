# HashMap vs HashTable

## HashMap과 HashTable의 차이

HashMap과 HashTable이 제공하는 기능은 같습니다.&#x20;

이들을 정의한다면 **키에 대한 해시 값을 사용하여 값을 저장하고 조회하며, 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array**라고 할 수 있습니다. 이 associate array를 지칭하는 다른 용어가 있는데, 대표적으로 Map, Dictionary, Symbol Table 등입니다.

HashMap과 HashTable 사이에는 약간의 차이점이 있는데 이는 아래의 표와 같습니다.

|                                               | HashTable | HashMap |
| :-------------------------------------------: | :-------: | :-----: |
|                  Thread-Safe                  |     O     |    X    |
|                  Synchronize                  |     O     |    X    |
|                     Speed                     |   Slower  |  Faster |
|             nullable (key, value)             |     X     |    O    |
| <p>Additional Hash Function<br>(보조 해시 함수)</p> |     X     |    O    |

###

### Thread-Safety & Speed

#### HashTable 클래스

```java
public class Hashtable<K, V> extends Dictionary<K, V> implements Map<K, V>, Cloneable, java.io.Serializable {
    public synchronized int size() { }
    
    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }
    
    public synchronized V put(K key, V value) { }
}
```

Hashtable 클래스의 대부분의 API를 보면 위와 같이 메소드 전체에 synchronized 키워드가 존재하는 것을 볼 수 있습니다. (synchronized method, 메소드 전체가 임계 구역으로 설정됩니다.)

{% content-ref url="synchronized.md" %}
[synchronized.md](synchronized.md)
{% endcontent-ref %}

그렇기 때문에 Multi-Thread 환경에서는 나쁘지 않을 수도 있습니다.

하지만 동시에 작업을 하려고 해도 객체마다 Lock을 하나씩 가지고 있기 때문에 동시에 여러 작업을 해야할 때 병목현상이 발생할 수 밖에 없습니다. (메소드에 접근하게 되면 다른 쓰레드는 Lock을 얻을 때까지 기다려야 하기 때문입니다.)

Hashtable 클래스는 Thread-safe 하긴 하지만, 위와 같은 특징 때문에 멀티쓰레드 환경에서 사용하기에도 살짝 느리다는 단점이 있습니다. 또한 Collection Framework가 나오기 이전부터 존재하는 클래스이기 때문에 최근에는 잘 사용하지 않는 클래스입니다.



#### HashMap 클래스

```java
public class HashMap<K, V> extends AbstractMap<K, V> implemets Map<K, V>, Clonable, Serializable {
    public V get(Object key) { }
    public V put(K key, V value) { }
}
```

HashMap 클래스를 보면 synchronized 키워드가 존재하지 않습니다. 그렇기 때문에 Map 인터페이스를 구현한 클래스 중에서 성능이 제일 좋다고 할 수 있습니다. 하지만 synchronized 키워드가 존재하지 않기 때문에 Multi-Thread 환경에서 사용할 수 없다는 특징을 가지고 있습니다.

멀티 쓰레드 환경이 아니라면 HashMap을 사용하기에 대체적으로 적합하겠지만, 멀티 쓰레드 환경이라면 HashMap 클래스도 Hashtable 클래스의 대안이 될 수는 없습니다.

그러면 Hashtable 클래스보다는 더 빠르고 Multi-Thread 환경에서 쓸 수 있는 클래스는 없을까요?

➔ ConcurrentHashMap!

{% content-ref url="../concurrenthashmap/" %}
[concurrenthashmap](../concurrenthashmap/)
{% endcontent-ref %}

****
