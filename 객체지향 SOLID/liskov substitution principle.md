리스코프 치환 원칙(Liskov Substitution Principle, LSP)은 객체 지향 프로그래밍의 중요한 원칙 중 하나입니다. 이 원칙은 서브타입(subtype)을 기반 클래스(superclass)로 대체할 때, 그 대체가 원래 클래스의 기능을 손상시키지 않아야 한다는 개념을 나타냅니다. 즉, 서브클래스는 부모클래스의 기능을 적절히 확장하거나 변경할 수 있지만, 부모클래스에서 정의된 기본 규약을 준수해야 합니다. 이것은 프로그램의 안전성과 확장성을 보장하기 위해 중요한 원칙 중 하나입니다.
(https://www.youtube.com/watch?v=_G50Joyys_E)


이 예시에서는 `Bird` 클래스를 상속받는 `Penguin` 클래스를 정의합니다. 일반적으로 `Bird`는 `fly` 메소드를 가지고 있지만, `Penguin`은 실제로 날 수 없습니다. 이러한 설계는 `Bird` 객체를 `Penguin` 객체로 대체할 수 없음을 의미하며, LSP를 위반합니다.
```java
class Bird {
    public void fly() {
        System.out.println("This bird is flying.");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguin can't fly.");
    }
}

public class Main {
    public static void main(String[] args) {
        Bird bird = new Penguin();
        bird.fly(); // UnsupportedOperationException 발생
    }
}


```

이 예시에서는 각각의 동물 타입에 대해 독립적인 클래스를 만들고 공통의 인터페이스 또는 추상 클래스를 사용합니다. 이렇게 함으로써 모든 동물 타입은 `Animal` 인터페이스를 대체할 수 있으며, 각 클래스는 자신의 행동을 정의합니다.
```java
interface Animal {
    void makeSound();
}

class Bird implements Animal {
    public void makeSound() {
        System.out.println("The bird is chirping.");
    }

    public void fly() {
        System.out.println("The bird is flying.");
    }
}

class Penguin implements Animal {
    public void makeSound() {
        System.out.println("The penguin is honking.");
    }
    
    public void swim() {
        System.out.println("The penguin is swimming.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal bird = new Bird();
        Animal penguin = new Penguin();

        bird.makeSound(); // "The bird is chirping."
        // bird.fly(); // 가능 (bird가 Bird 타입일 경우)

        penguin.makeSound(); // "The penguin is honking."
        // penguin.swim(); // 가능 (penguin이 Penguin 타입일 경우)
    }
}

```



```java
```


```java
```