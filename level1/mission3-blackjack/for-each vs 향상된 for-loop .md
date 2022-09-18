# for-each vs 향상된 for-loop

[https://jaehun2841.github.io/2019/02/16/effective-java-item45/#Stream을-과도하게-쓴-코드](https://jaehun2841.github.io/2019/02/16/effective-java-item45/#Stream%EC%9D%84-%EA%B3%BC%EB%8F%84%ED%95%98%EA%B2%8C-%EC%93%B4-%EC%BD%94%EB%93%9C)

[https://tecoble.techcourse.co.kr/post/2020-05-14-foreach-vs-forloop/](https://tecoble.techcourse.co.kr/post/2020-05-14-foreach-vs-forloop/)

이펙티브자바 item45. 스트림은 주의해서 사용하라.

스트림의 forEach 연산을 종단연산으로만 사용해야하는 이유?

자바의 stream은 컬렉션과 같은 자료형의 요소를 하나씩 참조해 함수형 인터페이스(람다식)를 통해 반복적인 작업의 처리를 가능하게 해준다. 이 stream에는 종단연산인 

### Problem

자바에서 Stream은 컬렉션 등의 요소를 하나씩 참조해 함수형 인터페이스(람다식)를 통해 반복적인 작업의 처리를 가능하게 해준다. Stream이 반복적인 일의 처리가 가능하므로, 반복문(for-loop 등)을 대신해 Stream을 사용하는 경우가 많다.

**forEach 연산은 최종 연산 중 기능이 가장 적고 가장 ‘덜’ 스트림답기 때문에, forEach 연산은 스트림 계산 결과를 print하는데에만 사용하고 계산하는 데는 쓰지 말자**

본래 forEach는 스트림의 종료를 위한 연산이다.

로직을 수행하는 역할은 Stream을 반환하는 중간연산이 해야하는 일이다.

Stream.forEach() 대신 **향상된 for문**
을 사용해도 충분히 가독성 좋은 코드가 될 수 있다.

****Stream forEach는 loop가 아니다.****

```java
List<Integer> list = new ArrayList<>();

for (int i = 1; i < 101; i++) {
    list.add(i);
}

list.forEach(i -> {
    if (i > 50) {
        System.out.println("여기 들어옴 " + i);
        return;
        //각 수행에 대해 다음 수행을 막을 뿐, 100번 모두 조건을 확인한 후에야 종료한다.
    }
    System.out.println(i);
});
```

```java
List<Integer> list = new ArrayList<>();

for (int i = 1; i < 101; i++) {
    list.add(i);
}

list.stream().forEach(i -> {
    if (i > 50) {
        System.out.println("여기 들어옴 " + i);
        return;
        //각 수행에 대해 다음 수행을 막을 뿐, 100번 모두 조건을 확인한 후에야 종료한다.
    }
    System.out.println(i);
});
```

향상된 for-loop

1.간편한,가독성 좋은 코드

2. 배열 인덱스 문제 해결 (ArrayIndexOutOfBoundsException 예외를 피할 수 있다.)

**for** (자료형 한 단계 아래의 자료형의 변수명 : 배열명) {

}

얘는 위에와 같은 문제가 생기지 않는다.

forEach는 어떨 때 조심해야 하나???

****종료 조건이 있는 로직을 구현할 때 주의해야 한다.****

[https://tecoble.techcourse.co.kr/post/2020-05-14-foreach-vs-forloop/](https://tecoble.techcourse.co.kr/post/2020-05-14-foreach-vs-forloop/)

*이펙티브 자바 아이템 46*
에 따르면, **forEach 연산은 최종 연산 중 기능이 가장 적고 가장 ‘덜’ 스트림답기 때문에, forEach 연산은 스트림 계산 결과를 보고할 때(주로 print 기능)만 사용하고 계산하는 데는 쓰지 말자**

**아이템 46. 스트림에서는 부작용 없는 함수를 사용하라**