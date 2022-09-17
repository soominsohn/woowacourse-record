# [AssertJ Exception Assertions] 예외를 테스트하는 방법

AssertJ Exception Assertions 을 이용해 예외를 테스트해보자.

---

### **Background**

```java
public void checkName(String name) {
	if (!isValid(name)) {
            throw new IllegalArgumentException();
    }
}

```

유효성을 검증하는 함수를 구현했다. 근데 과연 이 함수가 적절히 예외를 던져주는지 확인하고 싶다. 어떻게 해야 할까?

---

### **AssertJ 로 예외를 테스트하기**

방법이 두가지 있다.

- assertThatThrownBy(ThrowingCallable)을 사용하는 방법
- assertThatExceptionOfType(예외종류.class)를 사용하는 방법
1. **먼저 assertThatThrownBy()를 사용하는 방법이다.**

```java
import static org.assertj.core.api.Assertions.*;

assertThatThrownBy(() -> {
    //여기서 예외를 던진다.
		//checkName("invalidName");
}).isInstanceOf(IllegalArgumentException.class)
  .hasMessageContaining("something");

```

람다식 안에서 예외가 발생하면, 그 예외를 캡쳐해온다.캡쳐해온 예외의 instance가 무엇인지 isInstanceOf()로 검사를 한다.예외 종류가 일치하면 테스트 통과! hasMessageContaining("something"); 는 에러메세지가 “something”을 포함하는지 확인하는 것이다.

1. **다음으로 assertThatExceptionOfType(예외종류.class)를 사용하는 방법이다.**

```java
import static org.assertj.core.api.Assertions.assertThatExceptionOfType;

assertThatExceptionOfType(IllegalArgumentException.class)
  .isThrownBy(() -> {
     //여기서 예외를 던진다.
		//checkName("invalidName");
}).withMessageMatching("something");

```

돌아가는건 비슷하다. 다만 순서가 다를뿐이다.먼저 어떤 예외 종류를 검사하고 싶은지assertThatExceptionOfType()로 적는다.그 다음에 isThrownBy(() → {}) 여기 안에 예외를 던져준다.던져진 예외가 assertThatExceptionOfType()의 예외와 일치하면 테스트 성공!여기서 메세지 확인은 withMessageMatching을 사용한다.

**+ 따로 메써드를 제공하는 예외들**

- assertThatIllegalArgumentException()
- assertThatIllegalStateException()
- assertThatIOException()
- assertThatNullPointerException()

```java
public void testException() {
   assertThatIllegalStateException().isThrownBy(() -> {
													//여기서 예외를 던진다.
													//checkName("invalidName"); })
                          .withMessageContaining("something")
                          .withNoCause();
}

```

이렇게 사용하면된다.

### **예외 안날리는 경우도 테스트하고 싶은데요?**

가능하다.

```java
assertThatCode(() -> {
  // 예외를 안던지는 코드 자리
	//checkName("validName");
}).doesNotThrowAnyException();

```

ref.

**[https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#exception-assertion](https://joel-costigliola.github.io/assertj/assertj-core-features-highlight.html#exception-assertion)**