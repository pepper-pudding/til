---
description: HashMap은 정렬이 되지 않고, TreeMap은 정렬이 된다.
---

# HashMap vs TreeMap

## 차이점

|                                      | HashMap                                                  | TreeMap  |
| ------------------------------------ | -------------------------------------------------------- | -------- |
| <mark style="color:red;">Sort</mark> | X <mark style="color:green;">(Use LinkedHashMap!)</mark> | O        |
| add() time complexity                | O(1)                                                     | O(log n) |
| get() time complexity                | O(1)                                                     | O(log n) |
| contiansKey() time complexity        | O(1)                                                     | O(log n) |



## 유사점

|             | HashMap | TreeMap |
| ----------- | ------- | ------- |
| Thread-safe | X       | X       |
|             |         |         |
|             |         |         |
