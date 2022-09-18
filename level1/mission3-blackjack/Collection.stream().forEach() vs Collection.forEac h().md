# Collection.stream().forEach() vs Collection.forEach()

1. 순서
    - Collection.forEach는 컬렉션의 iterator를 사용한다. 따라서 순서가 명확하다.
    - 반면 Stream.forEach는 순서가 명확하지 않다. 순차스트림이라면 보통 순서대로 실행되지만 병렬스트림이라면 순서가 보장되지 않는다.
2. Collection 수정과 Side Effect
    - Collection.forEach는 iter동안 구조적으로 변경되어선 안된다. 순회하는 동안 요소가 추가/삭제되면 ConcurrentModification Execption이 발생한다. 동시에 컬렉션은 fail-fast하게 설계되었다. 즉 변경이 발생하면 바로 예외가 던져진다.
    - 한편 Stream.forEach는 스트림 파이프라인동안 요소가 추가/삭제되면 마찬가지로 ConcurrentModification Exception을 발생시키지만 나중에 던져진다.
    - 추가로, Collection.forEach는 iter동안 요소값이 변경되는것이 허용되지만 Stream.forEach는 non-interfering을 요구. Stream.forEach에서도 값변경이 되긴하지만 권장되지않는다. 예상치못한 결과로 이어질수있음.
3. 동기화
    - synchronized collection을 순회할때, Collection.forEach는 락을 걸고 Stream.forEach는 spliterator를 사용해서 Lock을 걸지 않는다.

### 결론

사실 차이는 미미하다. 특별한 요구사항이 있는게 아니라면 그냥 Collection.forEach를 사용하자. 굳이 Collection.Stream.forEach말고