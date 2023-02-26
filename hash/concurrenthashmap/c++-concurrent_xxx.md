# C++ Concurrent\_XXX

There is tbb::concurrent\_hash\_map and tbb::concurrent\_unordered\_map of [Intel TBB](https://www.threadingbuildingblocks.org/)

## concurrent\_hash\_map

count, find, insert, erase 동작은 Thread-Safe하다. count, find, insert, erase는 기존의 iterator를 무효화시킬 수 있다.\


count, find, insert, erase를 제외한 모든 메서드가 Thread-Safe하지 않다. 즉, 삽입/삭제가 일어날 수 있는 도중의 iteration이 Thread-Safe하지 않다.\
\


여러 스레드에서의 concurrent access를 위해 accessor를 사용한다. accessor는 **reader-writer spin lock**을 사용하며, find, insert, erase에 사용된다.\
\
map 자료 구조의 주요 동작 (삽입/조회/삭제/순회) 중 순회 기능이 Thread-Safe하지 않다. 따라서, 삽입/조회/삭제 뿐 아니라, **iteration (Not Thread-Safe)** 역시 주요 기능 중 하나라면, 아쉽지만 락을 사용하는 방식으로 전환해야 한다.\
\
아래 표는 hash\_map의 주요 함수들에 대해 쓰레드 세이프 여부에 대해 정리한 것이다.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

## concurrent\_unordered\_map



## 참고자료

{% embed url="http://egloos.zum.com/sweeper/v/3053914" %}

{% embed url="http://egloos.zum.com/sweeper/v/3053901" %}
