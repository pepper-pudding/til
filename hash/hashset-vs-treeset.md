---
description: HashSet은 정렬이 되지 않고, TreeSet은 정렬이 된다.
---

# HashSet vs TreeSet

## 차이점

|                                      | HashSet | TreeSet  |
| ------------------------------------ | ------- | -------- |
| <mark style="color:red;">Sort</mark> | X       | O        |
| base                                 | HashMap | TreeMap  |
| add() time complexity                | O(1)    | O(log n) |
| remove() time complexity             | O(1)    | O(log n) |

## 유사점

|                    | HashSet | TreeSet |
| ------------------ | ------- | ------- |
| Thread-safe        | X       | X       |
| fail-fast iterator | O       | O       |
