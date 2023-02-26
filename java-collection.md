# Java Collection 시간복잡도 총정리

## Map

|                   |   get()   | containsKey() |  next()  |
| ----------------- | :-------: | :-----------: | :------: |
| HashMap           |    O(1)   |      O(1)     |  O(h/n)  |
| LinkedHashMap     |    O(1)   |      O(1)     |   O(1)   |
| TreeMap           | O(log  n) |    O(log n)   | O(log n) |
| ConcurrentHashMap |    O(1)   |      O(1)     |  O(h/n)  |



## Set

|               |   add()  | contains() |  next()  |
| ------------- | :------: | :--------: | :------: |
| HashSet       |   O(1)   |    O(1)    |  O(h/n)  |
| LinkedHashSet |   O(1)   |    O(1)    |   O(1)   |
| TreeSet       | O(log n) |  O(log n)  | O(log n) |



## List

|            | add() | remove() | get() | contains() |
| ---------- | :---: | :------: | :---: | :--------: |
| ArrayList  |  O(1) |   O(n)   |  O(1) |    O(n)    |
| LinkedList |  O(1) |   O(1)   |  O(n) |    O(n)    |

##

## Queue

|                       | offer() | peak() | poll() | size() |
| --------------------- | :-----: | :----: | :----: | :----: |
| ArrayDeque            |   O(1)  |  O(1)  |  O(1)  |  O(1)  |
| ConcurrentLinkedQueue |   O(1)  |  O(1)  |  O(1)  |  O(n)  |



## 참고자료

{% embed url="https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html" %}

{% embed url="https://hbase.tistory.com/185" %}

{% embed url="https://www.grepiu.com/post/9" %}

{% embed url="https://goodteacher.tistory.com/112" %}
