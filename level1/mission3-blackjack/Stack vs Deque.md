# Stack vs Deque

# **Stack vs Deque**

왜 자바에서는 Stack 대신 Deque를 써야할까?

- Java의 Stack 은 Vector를 상속받는다.
- Vector는 멀티스레드 환경의 여부와 상관없이 대부분의 조건에서 성능 저하를 일으킨다.
- 동기화로인한 성능 문제!!!

**Vector**

- thread-safe하다.
- 그러므로 멀티 스레드 환경에서 안전하게 객체를 추가하고 삭제할 수 있습니다.
- But!!! 스레드가 1개일 때도 동기화하기 때문에 ArrayList보다 성능이 떨어진다.
- 그냥 get 하는데도 동기화 사용함 (키워드로 확인)
- ![synchronized.png](untitled.png)
     



**ArrayList**

- 자동 동기화 X
- 동기화 옵션 O (Collections.synchronizedList()를 넘겨주게되면 thread-safe한 ArrayList를 사용할 수 있다.)
- 벡터보다 속도가 빠르다.

**Stack**

- It doesn't have support for setting the initial capacity
- It uses locks for all the operations. This might hurt the performance for single threaded executions.
- Vector를 상속받음
- thread-safe, but! 성능 저하

![Untittled 1.png](Untitled%201.png)

synchronized keyword가 붙는다!!! 두둥

**Deque (ArrayDeque 구현체 사용)**

- 모든 스택 작업을 제공
- lock을 사용하지 않는다.
- Stack 클래스의 초기 용량 설정 부족 문제를 해결
    - 생성자로 배열의 초기 크기를 지정할 수 있다.
    - 용량이 초과하면 기존 용량의 2배로 늘려주거나 줄여주는 로직 존재

Deque는 어떤 구현체를 사용하는 것이 좋을까?

**ArrayDeque vs LinkedList**

- ArrayDeque 은 Deque 인터페이스의 구현체이며 크기가 재설정을 할 수 있다.

**ArrayDeque**

- Array의 장, 단점 있다고 보면 됨
- 크기 재설정 가능
- null 요소 추가 불가능
- 양쪽 끝에서 추가, 제거가 효율적

**LinkedList**

- linked list의 장, 단점 있다고 보면 됨
- List의 구현체
- null 요소 추가 가능
- 반복중에 현재 요소를 제거하는 것이 효율적
- LinkedList 는 ArrayDeque 보다 더 많은 메모리를 소모한다.
- size의 변동성이 매우 큰 경우 즉각적인 메모리 공간 확보를 위해선 LinkedList방식이 적절

Deque는 stack과 달리 thread-safe하지 않다.

그러므로 **멀티스레드 환경**에서는 스레드로부터 안전한 Deque 을 사용해야한다. 아래 두개 중 하나를 사용하면 된다.

**멀티스레드 환경에서는** **LinkedBlockingDeque**와 **ConcurrentLinkedDeque**를 사용

**LinkedBlockingDeque**

- 잠금 메커니즘을 사용하여 한 번에 단일 스레드에서만 데이터를 작동할 수 있도록 할 때 사용한다.

**ConcurrentLinkedDeque**

- 각각의 스레드가 공유 데이터에 액세스 할 수 있도록 할 때 사용한다. 또한 데이터를 작동할 때 성능에 영향을 미칠 수 있다는 차이점이 있다.
- *[ConcurrentLinkedDeque](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/ConcurrentLinkedDeque.html)* is a lock-free implementation of *Deque* interface. **This implementation is completely thread-safe** as it uses an [efficient lock-free algorithm.](http://www.cs.rochester.edu/~scott/papers/1996_PODC_queues.pdf)

멀티스레드 환경에서는 **ConcurrentLinkedDeque**를 사용하는게 좋고, 스택의 size가 제한되어있는 경우에 **LinkedBlockingDeque**를 사용하는 것을 고려해볼만 하다.

**결론**

대부분의 상황에서는 Deque 을 사용할 때 LinkedList 보다는 ArrayDeque 을 사용하면 된다.

또한 멀티스레드 환경에서 단일스레드를 사용할 계획이라면 **LinkedBlockingDeque**를 사용하고

멀티스레드를 사용할 계획이라면 **ConcurrentLinkedDeque**를 사용하면 된다.