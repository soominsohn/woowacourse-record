# 자바의 final 키워드

### **세 줄 요약**

- 자바의 final 키워드는 변수, 메소드, 클래스 앞에 사용된다.
- 자바의 final 키워드는 사용되는 곳에 따라 해석이 다르다.
- 그러나 공통적으로 가지는 의미는 앞에 final 키워드를 붙여 다른사람의 사용을 제한한다는 의미를 가지고 있다.

**final 키워드가 붙는 곳에 따라 제한하는 내용을 표로 간략히 정리하였다.**

그럼 final 키워드가 각각 변수, 메소드, 클래스 앞에 붙을 경우 어떤 의미를 갖는지 알아보자.

### **final이 변수앞에 붙을 때**

변수 앞에 final을 붙이면 변수는 재할당이 불가능하다는 의미를 갖는다.

변수가 기본형 변수인지, 참조형 변수인지에 따라 변경 가능한 범위가 다르다.

- 기본형 변수는 값을 변경하지 못한다.
- 참조형 변수는 객체 내부의 값을 변경할 수 있지만, 가리키는 객체는 변경할 수 없다.

```arduino
public class finalExample1 {

    public static void main(String[] args) {

        final int number = 10;

//Compile Error//number = 5;

        System.out.println("number: " + number);

//Car 객체(참조형 변수) final로 생성
        final Car car = new Car("myCar", 10000);

//final 참조형 변수는 가리키는 객체를 변경할 수 없다.//Compile Error//car = new Car("yourCar", 5000);//final 참조형 변수는 객체 내부의 값이 변경 가능하다.
        car.setName("yourCar");

        System.out.println(car);

    }

}

class Car {

    private String name;
    private int price;

    public Car(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {

        return "name: " + name + ", price: " + price;
    }

}

```

또한, 변수 앞에 final을 붙이면 변수는 **수정이 불가능**하다는 의미를 갖는다. 수정이 불가능하므로, **초기화가 필수적**이다.

변수를 final로 선언하였을 때, 초기화 전에 사용한다면 컴파일 에러가 발생한다.

final 변수가 선언된 위치에 따라 각각 초기화하는 방법이 다르다.

- **메소드안의 변수에 final을 붙였을 때 초기화 하는 방법**
    1. 선언 시 초기화
    2. 사용하기 전 초기화

**1. 선언 시 초기화**

```arduino
public class finalExample2 {

    public static void main(String[] args) {

				someMethod();

    }

    public static void someMethod() {
        final int number = 5;

        System.out.println(number);

    }

}

```

**2. 사용하기 전 초기화**

```arduino
public class finalExample2 {

    public static void main(String[] args) {

				someMethod();

    }

    public static void someMethod() {
        final int number;

        number = 5;

        System.out.println(number);

    }

}

```

초기화 전에 사용하면 컴파일 오류가 발생한다.

```java
public class finalExample2 {

    public static void main(String[] args) {

				someMethod();

    }

    public static void someMethod() {
        final int number;

//Compile Error 발생
        System.out.println(number);

				number = 5;

    }

}
```

- **객체의 멤버변수에 final을 붙였을 때 초기화 하는 방법**
    1. 선언시 초기화
    2. 생성자를 이용한 초기화
    3. 초기화 block을 이용한 초기화

**1. 선언 시 초기화**

```java
class Car {

    private final String name = "myCar";
    private final int price = 10000;

    @Override
    public String toString() {

        return "name: " + name + ", price: " + price;
    }

}

```

**2. 생성자를 이용한 초기화**

```arduino
public class finalExample3 {

    public static void main(String[] args) {

        Car car = new Car("myCar", 10000);

        System.out.println(car);

    }

}

class Car {

    private final String name;
    private final int price;

    public Car(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public String toString() {

        return "name: " + name + ", price: " + price;
    }

}

```

**3. 초기화 block을 사용한 초기화**

```java
class Car {

    private final String name;
    private final int price;

    {
        name = "myCar";
        price = 10000;

    }

    @Override
    public String toString() {

        return "name: " + name + ", price: " + price;
    }

}

```

### 

### 

### 

### 

### **final이 method 앞에 붙을 때**

final 키워드는 변수뿐만 아니라 method앞에도 붙일 수 있다.

final 키워드가 method앞에 붙었을 때, 그 의미는 subclass에서 override가 불가능하다는 뜻이다.

만일 subclass에서 override하려고 하면, 컴파일 오류가 발생한다.

- subclass에서 override가 불가능하다.
- 상속 받은 그대로 사용해야 한다.

subclass에서 override가 불가능하다.

```java
class A {
    public final void someMethod() {
        System.out.println("override가 불가능한 method입니다.");
    }
}

class B extends A {

//Compile Error 발생@Override
    public final void someMethod() {
        System.out.println("override하고 싶어요,,");
    }
}
```

상속받은 그대로 사용해야 한다.

```scala
public class finaleExample4 {

    public static void main(String[] args) {

        B b = new B();
        b.someMethod();

    }

}

class A {
    public final void someMethod() {
        System.out.println("override가 불가능한 method입니다.");
    }
}

class B extends A {

}

```

### 

### 

### **그럼 왜 method앞에 final 키워드를 붙일까?**

상속받았을 때, 수정하면 안되는 메소드에 붙인다.

예를 들어 Thread의 isAlive() method 같은 경우 쓰레드가 살아있는지 죽어있는지 확인해주는 method이다.

그러나 이 method를 상속받은 클래스에서 override하여 사용하게 되면 어떻게 될까? 매우 난처해질 것이다.

그렇기 때문에 final키워드를 사용하여 상속받은 클래스에서 override하지 못하게 제한하는 것이다.

### **final이 클래스 앞에 붙을 때**

- 상속이 불가능하다.
- subclass를 만들 수 없다.

```scala
final class A {
    public void someMethod() {
        System.out.println("override가 불가능한 method입니다.");
    }
}

//Comple Error!class B extends A {

}

```

### 

### **왜 class 앞에 final 키워드를 붙이는 것일까?**

그렇다면 어떠한 경우에, final class를 생성하여 상속을 막는 것일까?

- Integer, Float과 같은 Wrapper 클래스의 상속을 막기 위해
- String, Boolean, Integer, Float, Long과 같은 immutable class 를 만들기 위해서.

**Integer, Float 등의 wrapper class 는 final class여서 상속이 불가능하다.**

```scala
//Comile Errorclass myInteger extends Integer {

}

```

즉, 어떤 클래스를 상속받아 하위 클래스에서 무분별하게 사용하는 것을 막고 싶을 때 사용한다.

**String과 같은 클래스는 대표적인 immutable class이다.  Immutable class란?**

- heap영역에서 변경 불가능한 객체이다. 재할당만 가능하다. 즉, String a = “something”; 에서 String a = “changed”로 재할당이 가능하다.
- “something”이라는 값이 “changed”로 변경하는 것이 아니라, heap영역에 새로운 객체를 만들어 그 객체를 a가 참조하게 하는 것이다.
- final keyword를 붙이지 않으면 immutable class를 만들 수 없다.

### **마무리**

### **그래서 결국 final 키워드는 왜 쓰는 것인가?**

지금까지 final 키워드가 각각 변수, method, 클래스 앞에 붙을 때 어떻게 사용되는지, 왜 사용하는지에 대하여 알아보았다.

final 키워드는 주로 변수, 메소드, 클래스 앞에 붙는다.

final 메소드와 클래스는 주로 라이브러리 형태의 프로젝트를 만들 때 사용된다.

라이브러리를 제대로 이해하지 못하고 상속받아 재정의하여 사용한다면 큰일이 나겠죠?

그렇기 때문에 final 키워드를 붙여 무분별한 사용을 막는 것이다.

개발을 할 때, 자신이 만든 메서드와 클래스를 다른사람이 사용하지 못하게 막고 싶을 때 사용하면 되겠다.

ref.

[https://www.geeksforgeeks.org/final-keyword-in-java/](https://www.geeksforgeeks.org/final-keyword-in-java/)

[https://www.scaler.com/topics/java/final-keyword-in-java/#advantages-and-disadvantages-of-final-variable-in-java](https://www.scaler.com/topics/java/final-keyword-in-java/#advantages-and-disadvantages-of-final-variable-in-java)

[https://library1008.tistory.com/1](https://library1008.tistory.com/1)

[https://sabarada.tistory.com/148](https://sabarada.tistory.com/148)

| --- | --- |