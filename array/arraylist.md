# ArrayList

{% embed url="https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html" %}

## 동적 배열

ArrayList는 표준 배열보다는 느리지만 배열에서 많은 조작이 필요한 경우 유용하게 사용할 수 있습니다. ArrayList는 객체가 추가되어 용량을 초과하면 자동으로 용량이 늘어납니다. 2배씩 증가하면서 용량이 늘어납니다.



## ArrayList의 구현

### 용량을 초과하는 경우 부족한 크기만큼 용량을 늘리는 과정은 어떻게 진행될까?

_cpp의 vector와 java의 ArrayList는 비슷해보인다. vector도 동적 배열인데, 용량 초과 시 두 배 크기에 해당하는 메모리를 할당받은 다음 기존 vector의 값들을 copy한다고 알고 있다. 더 효율적으로 구현하기 위해 copy 대신 move를 사용하는 경우도 있다고 들었던 것 같은데 잘 모르겠다. java도 비슷할까?_

__

```java
class ArrayList {
    private Object[] data;
    private int size;
    private int index;
    
    public ArrayList() {
        this.size = 1;
        this.data = new Object[this.size];
        this.index = 0;
    }
    
    public void add(Object obj) { // O(1)
        System.out.println("index: " + this.index + ", size: " + this.size + ", data size: " + this.data.length);
        if(this.index == this.size - 1) {
            doubling();
        }
        data[this.index] = obj;
        this.index++;
    }
    
    private void doubling() { 
        this.size = this.size * 2;
        Object[] newData = new Object[this.size];
        for(int i = 0; i < data.length; i++) {
            newData[i] = data[i];
        }
        this.data = newData;
        System.out.println("doubling >>> index: " + this.index + ", size: " + this.size + ", data size: " + this.data.length);
    }
    
    public Object get(int i) throws Exception { // O(1)
        if(i > this.index - 1) {
            throw new Exception("ArrayIndexOutOfBound");
        } else if (i < 0) {
            throw new Exception("Negative Value");
        }
        return this.data[i];
    }
    
    public void remove(int i) throws Exception { // O(n)
        if(i > this.index - 1) {
            throw new Exception("ArrayIndexOutOfBound");
        } else if (i < 0) {
            throw new Exception("Negative Value");
        }
        System.out.println("data removed: " + this.data[i]);
        for(int x = i; x < this.data.length - 1; x++) {
            data[x] = data[x + 1]; // 삭제 시 시간복잡도가 O(n)인 이유!
        }
        this.index--;
    }
}
```



## 참고자료

{% embed url="https://poetic-code.tistory.com/90" %}
