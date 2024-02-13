## 정의
지정한 스택 메모리 사이즈보다 더 많은 스택 메모리를 사용하게 되어 에러가 발생하는 상황


## 발생하는 상황
#### 1.재귀함수
```java
public long calculateFactorial(long number) {
    return number == 1 ? 1 : number * calculateFactorial(number - 1);
}
```

재귀함수를 호출할 때 파라미터,리턴 값, 복귀 주소등이 스택에 저장되는데 재귀함수 사용 시 호출 함수가 종료되지 않은채 계속해서 스택에 메모리가 쌓이면서 가용 메모리가 없을 경우 스택 오버 플로우가 발생 한다.

#### 2.상호 참조
두 클래스간에 생성을 위임하면서 체이닝을 이루게 되면 발생

```java
public class ClassOne {
    private int oneValue;
    private ClassTwo clsTwoInstance = null;

    public ClassOne() {
        oneValue = 0;
        clsTwoInstance = new ClassTwo();
    }
}

public class ClassTwo {
    private int twoValue;
    private ClassOne clsOneInstance = null;

    public ClassTwo() {
        twoValue = 10;
        clsOneInstance = new ClassOne();
    }
}
```

#### 3.본인 참조
클래스 내에서 본인 클래스를 다시 생성하게 되면 스택오버플로우 발생

```java
public class ClassOne {
    private int oneValue;
    private ClassTwo clsTwoInstance = null;

    public ClassOne() {
        oneValue = 0;
        clsTwoInstance = new ClassTwo();
    }
}

public class ClassTwo {
    private int twoValue;
    private ClassOne clsOneInstance = null;

    public ClassTwo() {
        twoValue = 10;
        clsOneInstance = new ClassOne();
    }
}
```


___
(https://incheol-jung.gitbook.io/docs/q-and-a/java/stw)

- 스택 오버플로우는 주로 재귀 호출이 무한하게 계속되는 상황에서 발생합니다.
- 예를 들어, 재귀 함수에서 종료 조건을 정의하지 않거나 재귀 호출이 종료 조건을 만족하지 못한 채 계속해서 호출되는 경우 스택 오버플로우가 발생합니다.
- 스택 오버플로우는 호출 스택(Call Stack)에 제한된 메모리 공간을 초과하는 경우에 발생하며, 주로 스택 프레임(Stack Frame)이 너무 많이 쌓이는 상황에서 발생합니다.
- 스택 오버플로우는 호출 스택 자체의 문제로 인한 오류로, 메모리 부족 상황과 직접적으로 관련이 없습니다.



## 추가적인 사항
버퍼 오버 플로우?
데이터를 저장하는 과정에서 그 데이터를 저장할 메모리 위치가 유효 한지를 검사하지 않아서 발생
따라서 스택 오버 플로우와 개념이 다르다.

오버플로우?
데이터가 지정된 크기의 공간보다 커서 해당 메모리 공간을 벗어 나는 경우 발생
```java
	public static void main(String[] args){
    	int x = 50000;
        int y = 50000;
        
        int z = x * y;
        System.out.println(z); // -1794967296
    }
```