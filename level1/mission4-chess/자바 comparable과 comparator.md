# 자바 comparable과 comparator

- 정렬 수행 시 기본적으로 적용되는 정렬 기준이 되는 메서드를 정의하는 인터페이스
1. package: java.lang.Comparable
2. Java에서 제공되는 정렬이 가능한 클래스들은 모두 Comparable 인터페이스를 구현하고 있으며, 정렬 시에 이에 맞게 정렬이 수행된다.

- 구현 방법
정렬할 객체에 Comparable interface를 implements 후, compareTo() 메서드를 오버라이드하여 구현한다.
- comparTo() 메서드 설명
    - 현재 객체 < 파라미터로 넘어온 객체: 음수 리턴
    - 현재 객체 == 파라미터로 넘어온 객체: 0 리턴
    - 현재 객체 > 파라미터로 넘어온 객체: 양수 리턴
    
    → 음수 또는 0 이면 객체의 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 바뀐다.
    

- 사용 방법
    - Arrays.sort(array)
    - Collections.sort(list)
    

Comparator도 있는데, Comparator는 왜 필요한가?

→ 그냥 Comparable 재정의해서 쓰면 안되나요?

만약 정렬 대상 클래스의 코드를 직접 수정할 수 없는 경우에는 어떻게 객체의 정렬 기준을 정의할 수 있을까요? 또는 정렬 하고자 하는 객체에 이미 존재하고 있는 정렬 기준과 다른 정렬 기준으로 정렬을 하고 싶을 때는 어떻게 해야할까요?

이 때 필요한 것이 바로 `Comparable`  인터페이스와 이름이 유사한 `Comparator`  인터페이스입니다. `Comparator` 인터페이스의 구현체를 `Arrays.sort()`나 `Collections.sort()`와 같은 정렬 메서드의 추가 인자로 넘기면 정렬 기준을 누락된 클래스의 객체나 기존 정렬 기준을 무시하고 새로운 정렬 기준으로 객체를 정렬할 수 있습니다.

```java
Comparator<Player> comparator = new Comparator<Player>() {
    @Override
    public int compare(Player a, Player b) {
        return b.getScore() - a.getScore();
    }
};

Collections.sort(players, comparator);
System.out.println(players); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

근데 이렇게 봐서는 조금 복-잡-하다. 코드의 양이 많다! 불-편-

## 람다 함수로 대체

`Comparator` 객체는 메서드가 하나 뿐인 함수형 인터페이스를 구현하기 때문에 람다 함수로 대체가 가능합니다.

```java
Collections.sort(players, (a, b) -> b.getScore() - a.getScore());
System.out.println(players); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

람다 함수를 사용하면 코드가 간결해짐을 알 수 있습니다.

```java
players.sort(Comparator.comparingInt(Person::getAge));
```

```java
Comparator<Person> personComparator = Comparator.comparing(Person::getName);
```

## Stream 으로 정렬

`Stream` 클래스의 `sorted()` 메서드도 `Comparator` 객체를 인자로 받아 정렬을 해줍니다. 스트림을 사용하면 위에서 살펴본 배열과 리스트의 정렬과 달리 기존 객체의 순서를 변경하지 않고, 새롭게 정렬된 객체를 생성하고자 할 때 사용됩니다.

```java
List<Player> sortedPlayers = players.stream()
        .sorted((a, b) -> b.getScore() - a.getScore())
        .collect(Collectors.toList());
System.out.println(sortedPlayers); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

**Comparable** : 객체 간의 **일반적인 정렬**이 필요할 때, Comparable 인터페이스를 확장해서 정렬의 기준을 정의하는 compareTo() 메서드를 구현한다.

**Comparator** : 객체 간의 **특정한 정렬**이 필요할 때, Comparator 인터페이스를 확장해서 특정 기준을 정의하는 compare() 메서드를 구현한다.