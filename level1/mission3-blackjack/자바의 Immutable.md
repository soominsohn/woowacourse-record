# 자바의 Immutable

현대의 프로그래밍 언어들은 불변객체이다.

그러나 자바는 본질적으로 가변객체이다.

그래서 자바에서도 불변객체를 만들기 위해 노력한다. 왜 그럴까?

그럼 불변 객체를 구현하는 프로그래밍 언어에서는 상속이 불가능할까?

자바와 immutable

불변 객체 (immutable object) 란 생성 이후 그 상태를 바꿀 수 없는 객체를 뜻한다.

불변 객체는 재할당은 가능하지만, 한번 할당하면 내부 데이터를 변경할 수 없다.

final과의 차이점?

final은 재할당이 불가능하다.

객체에 값을 할당하면 내부 데이터를 변경시킬 수 없다는 것입니다. 대표적인 예로 String, Integer, Boolean 등이 있습니다.

String의 경우에 

String str = “Hello”;

str = “modified”;

처럼 수정이 가능하여 변경가능 한 것 같아보이지만, str이 가리키는 객체의 참조값이 변경된 것일 뿐, 객체가 변경된 것이 아니다.  

아래는 가변객체이다.

```
class MutablePerson {
   public int age;
   public int name;

   public MutablePerson(int age, int name) {
    	this.age = age;
        this.name = name;
    }
}
```

아래는 불변객체이다.

그럼 이 클래스를 불변 클래스로 만들 수 있다.

```
class ImmutablePerson {
    private final int age;
    private final int name;

    public ImmutablePerson(int age, int name) {
	    	this.age = age;
        this.name = name;
    }
}
```

위와 같이 만들면 외부에서 **값을 수정할 수 없습니다. 따라서 불변객체가 됩니다.**(final 변수이므로 당연히 Setter 메서드를 작성할 수 없습니다)일반 객체들을 불변객체로 만드는 방법들은 뒤에 추가적으로 보여드리겠습니다.

# Immutable Object의 장단점

**장점**

- 객체에 대한 신뢰도가 높아집니다. 객체가 한번 생성되어서 그게 변하지 않는다면 transaction 내에서 그 객체가 변하지 않기에 우리가 믿고 쓸 수 있기 때문입니다.
- 생성자, 접근메소드에 대한 방어 복사가 필요없습니다.
- 멀티스레드 환경에서 동기화 처리없이 객체를 공유할 수 있습니다.

**단점**

- 객체가 가지는 값마다 새로운 객체가 필요합니다. 따라서 메모리 누수와 새로운 객체를 계속 생성해야하기 때문에 성능저하를 발생시킬 수 있습니다.

# Immutable Object 만들어보기

---

Immutable Object를 만드는 기본적인 아이디어는 필드에 `final`을 사용하고, `Setter`를 구현하지 않는 것입니다.이 아이디어는 불변객체의 필드가 모두 원시 타입일 경우에만 가능하고, 참조 타입일 경우엔 추가적인 작업이 필요합니다.

이제 필드가 원시 타입만 있는 객체와 참조 타입이 있는 객체를 불변객체로 만들어봅시다.

## 원시 타입만 있는 경우

**변경 전**

```
public class BaseObject {

    private int value;

    public BaseObject(final int value) {
        this.value = value;
    }

    public void setValue(int newValue) {
    	this.value = newValue;
    }

    // getter는 생략 했음
}
```

위 객체는 **불변객체가 아닙니다**. 앞서 설명드렸지만, 외부에서 Setter를 통해 int형 필드 value의 값을 변경할 수 있기 때문입니다.

필드가 원시 타입만 있으므로 이는 **`final` 키워드를 사용해서 불변객체로 만들 수 있습니다.**

**변경 후**

```
public class BaseObject {

    private final int value;

    public BaseObject(final int value) {
        this.value = value;
    }

    // getter는 생략 했음
}
```

필드에 `final` 키워드를 사용했으므로 Setter를 구현하는 것은 당연히 불가능합니다.따라서 이 객체의 value를 변경하려면 재할당하는 방법밖에 없습니다.

## 참조 타입이 있는 경우

참조 타입이 있는 경우의 대부분은 **final을 사용하고, Setter를 작성하지 않는 것으로는 불변객체를 만들 수 없습니다.**아래의 예를 보겠습니다.

```
public class Animal {

    private final Age age;

    public Animal(final Age age) {
        this.age = age;
    }

    public Age getAge() {
    	return age;
    }
}

class Age {

    private int value;

    public Age(final int value) {
        this.value = value;
    }

    public void setValue(final int value) {
        this.value = value;
    }

    public int getValue() {
    	return value;
    }
}
```

Animal 클래스는 final을 사용하고, Setter를 구현하지 않았지만 불변객체가 될 수 없습니다. 왜냐하면 Animal 클래스의 필드인 Age의 값을 아래처럼 변경할 수 있기 때문입니다.

```
public static void main(String[] args) {
    Age age = new Age(1);
    Animal animal = new Animal(age);

    System.out.println(animal.getAge().getValue());
    // Output: 1

    animal.getAge().setValue(10);
    System.out.println(animal.getAge().getValue());
    // Output: 10
}
```

즉, 불변 객체의 참조 변수 또한 불변이어야 합니다.필드에 참조 타입이 있는 경우는 여러가지 상황이 있을 수 있습니다. 대표적으로 (1)객체를 참조할 수도 있고, (2)Array나 (3)List 등을 참조할 수 있습니다.이 세가지 상황에 대해 불변객체를 만들어보겠습니다.

### (1) 참조 변수가 일반 객체인 경우

이 상황은 위의 예제를 고쳐보면 될 것입니다.따라서 이는 참조 변수인 Age도 불변객체로 만들어 해결할 수 있습니다.

```
public class Animal {

    private final Age age;

    public Animal(final Age age) {
        this.age = age;
    }

    // getter
}

class Age {

    private final int value;

    public Age(final int value) {
        this.value = value;
    }

    // getter
}
```

이 상황을 보면 결국 **"참조 변수도 불변 객체이어야 한다"**라는 결론이 나옵니다.

### (2) Array일 경우

```
public class ArrayObject {

    private final int[] array;

    public ArrayObject(final int[] array) {
        this.array = Arrays.copyOf(array,array.length);
    }

    public int[] getArray() {
        return (array == null) ? null : array.clone();
    }
}
```

배열일 경우에는 생성자에서 **배열을 받아 copy해서 저장하도록** 했고,getter를 **clone을 반환하도록** 하면 됩니다.배열을 **그대로 참조하거나, 그대로 반환할 경우 외부에서 배열 내부값을 변경시킬 수 있기 때문에, clone을 반환하게 되면 외부에서 값을 변경시킬 수 없습니다.**만약 **원시 타입 배열이 아니고, `Animal[]`과 같은 형태라면 해당 객체는 불변객체이어야 합니다.**

```
public static void main(String[] args) {
	int[] array = {1, 2, 3};
      	ArrayObject arrayObject = new ArrayObject(array);

        for (int num : arrayObject.getArray()) {
            System.out.print(num + " ");
        }
        // 결과: 1 2 3

        System.out.println();
        array[0] = 999; // arrayObject의 배열 값은 변하지 않는다.

        for (int num : arrayObject.getArray()) {
            System.out.print(num + " ");
        }
        // 결과: 1 2 3
}
```

### (3) List인 경우

List인 경우에도 Array와 마찬가지로 **생성시 생성자 인자를 그대로 참조하지 않고, 새로운 List를 만들어 값을 복사하도록 해야합니다.** 그리고 **getter를 통해 값 추가/삭제가 불가능하도록 Collection의 unmodifiableList 메서드를 사용했습니다.**

여기서 `Animal`은 앞서 만든 불변객체입니다.

```
import java.util.Collections;
import java.util.List;

public class ListObject {

    private final List<Animal> animals;

    public ListObject(final List<Animal> animals) {
        this.animals = new ArrayList<>(animals);
    }

    public List<Animal> getAnimals() {
        return Collections.unmodifiableList(animals);
    }
}
```

**결과**

```
public static void main(String[] args) {
    List<Animal> animals = new ArrayList<>();
    animals.add(new Animal(new Age(1)));

    ListObject listObject = new ListObject(animals);

    for (Animal animal : listObject.getAnimals()) {
        System.out.print(animal.getAge().getValue());
    }
    System.out.println();
    // Output: 1

    animals.add(new Animal(new Age(2))); // List인 animals에는 추가되지만 listObject의 List에는 추가되지 않는다

    for (Animal animal : listObject.getAnimals()) {
        System.out.print(animal.getAge().getValue());
    }
    System.out.println();
    // Output: 1
}

```

# 결론

- 불변객체는 한번 할당하면 필드 값을 변경할 수 없는 객체입니다.
- 하지만 재할당은 가능합니다.
- 필드가 원시 타입일 경우엔 **final** 사용으로 불변객체를 만들 수 있고, 참조 타입일 경우엔 추가적인 작업이 필요합니다.

불변객체를 사용하는 이유

## 요약

- 불변 객체에 대한 정의에는 차이가 있을 수 있으나, 불변 객체는 기본적으로 참조하고 있는 데이터를 변경할 수 없는 객체를 의미합니다. 불변 객체를 사용하였을 때 장점은 아래와 같습니다.외부에서 임의로 내부의 값을 제어할 수 없기 때문에객체의 자율성이 보장되고프로그램 내에서 변하지 않는 즉 고정된 부분이 많아짐으로써 프로그램 안정도를 높일 수 있습니다.
- 모든 객체를 불변으로 만들 필요는 없지만 변하지 않기를 바라는 객체를 불변객체로 만드는 데에 이 포스팅이 도움이 되셨기를 바랍니다.

**1. Thread-Safe하여 병렬 프로그래밍에 유용하며, 동기화를 고려하지 않아도 된다.**

멀티 쓰레드 환경에서 동기화 문제가 발생하는 이유는 공유 자원에 동시에 쓰기(Write) 때문이다. 하지만 만약 공유 자원이 불변의 자원이라면 어떻게 될까? 더 이상 동기화를 고려하지 않아도 될 것이다. 왜냐하면 항상 동일한 값을 반환할 것이기 때문이다. 이는 안정성을 보장할 뿐만 아니라 동기화를 하지 않음으로써 성능상의 이점도 가져다준다.

**2. 실패 원자적인(Failure Atomic) 메소드를 만들 수 있다.**

가변 객체를 통해 어떠한 작업을 하는 도중 예외가 발생하면 해당 객체가 불안정한 상태에 빠질 수 있다. 그리고 불한정한 상태를 갖는 객체는 또 다른 에러를 유발할 수 있다. 하지만 불변 객체라면 어떠한 예외가 발생하여도 메소드 호출 전의 상태를 유지할 수 있을 것이다. 그리고 예외가 발생하여도 오류가 발생하지 않은 것 처럼 다음 로직을 처리할 수 있다.

**3. Cache나 Map의 또는 Set 등의 요소로 활용하기에 더욱 적합하다.**

만약 캐시나 Map 또는 Set 등으로 사용되는 객체가 변경되었다면 이를 갱신하는 등의 작업을 추가로 해주어야 할 것이다. 하지만 객체가 불변이라면 한 번 데이터가 저장된 이후에 다른 부가 작업들을 고려하지 않아도 될 것이고, 이는 캐시나 다른 자료 구조를 사용하는데 용이하게 작용할 것이다.

**4. 부수 효과(Side Effect)를 피해 오류가능성을 최소화할 수 있다.**

부수 효과란 변수의 값이 변경되거나, 필드 값이 설정되는 등의 변화가 발생하는 효과를 의미한다. 만약 객체의 수정자(Setter)가 구현되어 있고, 여러 메소드에서 객체의 값이 변경된다면 객체를 예측하기 어려워질 것이다. 객체의 바뀐 상태를 파악하기 위해서는 메소드들을 살펴보아야 할 것이며 이러한 부분은 유지보수성을 상당히 떨어뜨린다. 그래서 이러한 부수효과가 없는 순수 함수들을 만드는 것이 상당히 중요한데, 객체가 불변이라면 어떻게 될까?

불변 객체는 기본적으로 값의 수정이 불가능하기 때문에 변경 가능성이 적으며, 객체의 생성과 사용이 상당히 제한된다. 그렇기 때문에 메소드들은 자연스럽게 순수 함수들로 구성될 것이고, 다른 메소드가 호출되어도 객체의 상태가 유지되기 때문에 안전하게 객체를 다시 사용할 수 있다. 이러한 불변 객체는 오류를 줄여 유지보수성이 높은 코드를 작성하도록 도와줄 것이다.

**5. 다른 사람이 작성한 함수를 예측가능하며 안전하게 사용할 수 있다.**

일반적으로 개발을 하면 다른 사람들과 협업을 하면서 진행하게 된다. 불변성(Immutability)은 협업을 하는 과정에서도 도움을 주는데, 불변성이 보장된 함수라면 다른 사람이 개발한 함수를 위험없이 이용할 수 있다. 마찬가지로 다른 사람도 내가 작성한 메소드를 호출하여도, 값이 변하지 않음을 보장받을 수 있다. 그렇기에 우리는 다른 사람의 코드를 변경에 대한 불안없이 이용할 수 있다. 또한 불필요한 시간을 절약할 수도 있는데, 이에 대한 예제는 아래에서 자세히 살펴보도록 하자.

**6. 가비지 컬렉션의 성능을 높일 수 있다.**

불변성(Immutability)을 활용하는 것은 많은 이점을 가져다주는데, 그 중에서 많은 사람들이 놓치는 것이 바로 GC의 성능을 높여준다는 것이다.

불변의 객체는 한번 생성된 이후에 수정이 불가능한 객체로, Java에서는 final 키워드를 사용하여 불변의 객체를 생성할 수 있다. 이렇게 객체를 생성하기 위해서는 객체를 가지는 또 다른 컨테이너 객체(ImmutableHolder)도 존재한다는 것인데, 당연히 불변의 객체(Object value)가 먼저 생성되어야 컨테이너 객체가 이를 참조할 수 있을 것이다. 즉, 컨테이너는 컨테이너가 참조하는 가장 젊은 객체들보다 더 젊다는 것(늦게 생성되었다는 것)이다. 이를 정리하면 다음과 같다.

1. Object 타입의 value 객체 생성
2. ImmutableHolder 타입의 컨테이너 객체 생성
3. ImmutableHolder가 value 객체를 참조

이러한 점은 GC가 수행될 때, 가비지 컬렉터가 컨테이너 객체 하위의 불변 객체들은 Skip할 수 있도록 도와준다. 왜냐하면 해당 컨테이너 객체(ImmutableHolder)가 살아있다는 것은 하위의 불변 객체들(value) 역시 처음에 할당된 그 상태로 참조되고 있다는 것을 의미하기 때문이다.