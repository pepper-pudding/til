# LinkedList

{% embed url="https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html" %}

LinkedList는 데이터가 연속된 위치에 저장되지 않고 모든 데이터가 데이터 부분과 주소 부분을 별도로 가지고 있습니다. 데이터는 포인터와 주소를 사용하여 연결합니다.

각 데이터는 노드라 불리며 배열에서 자주 삽입, 삭제가 이루어지는 경우 용이하여 ArrayList보다 선호됩니다.

하지만 ArrayList보다 검색에 있어서는 느리다는 단점이 있습니다.



## LinkedList의 구현

{% content-ref url="linkedlist-1.md" %}
[linkedlist-1.md](linkedlist-1.md)
{% endcontent-ref %}



## LinkedList의 종류

### 단일 연결 리스트(Singly Linked List)

각 노드가 다음 노드에 대해서만 참조하는 가장 단순한 형태의 연결 리스트입니다.\
일반적으로 큐를 구현할 때 사용됩니다.\
Head 노드를 잃어버려 참조하지 못하는 경우 데이터 전체를 사용 못하게 되는 단점이 있습니다.\
FAT 파일 시스템이 단일 연결 리스트로 파일 청크(동적 메모리로 할당되는 영역)를 연결합니다.



### 이중 연결 리스트(Doubly Linked List)

각 노드가 이전 노드, 다음 노드에 대해서 참조하는 형태의 연결 리스트입니다.\
삭제가 간편하며 단일 연결 리스트에 비해 데이터 손상에 강하지만, 관리할 참조가 2개이기 때문에 삽입이나 정렬의 경우 작업량이 더 많아집니다.



### 원형 연결 리스트(Circular Linked List)

연결 리스트에서 마지막 요소가 첫번째 요소를 참조합니다.\
스트림 버퍼의 구현에 많이 사용됩니다.

