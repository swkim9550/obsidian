
# 개념
___
  개방/폐쇄 원칙(Open/Closed Principle, OCP)은 소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다는 원칙을 나타냅니다. 이것은 새로운 기능을 추가할 때 기존 코드를 수정하지 않고 확장할 수 있어야 함을 의미합니다.
 (https://www.youtube.com/watch?v=EmnIdUvTRfk)


```java
class Animal {
    private String type;

    public Animal(String type) {
        this.type = type;
    }

    public String getType() {
        return type;
    }

    public void makeSound() {
        if (type.equals("Dog")) {
            System.out.println("멍멍!");
        } else if (type.equals("Cat")) {
            System.out.println("야옹!");
        } else {
            System.out.println("소리를 내지 못합니다.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Animal dog = new Animal("Dog");
        Animal cat = new Animal("Cat");

        dog.makeSound();
        cat.makeSound();
    }
}

```
이 코드에서 `Animal` 클래스는 동물의 종류를 나타내는 문자열 필드를 가지고 있으며, `makeSound()` 메서드에서 동물의 종류에 따라 다른 소리를 출력하고 있습니다.

이런 경우, 새로운 동물을 추가하거나 동물의 소리를 변경하려면 `Animal` 클래스를 수정해야 합니다. 이는 개방폐쇄원칙을 위반한 예시입니다. 새로운 동물을 추가하려면 `if-else` 블록을 계속 수정해야 하므로 코드의 확장성과 유지보수성이 떨어집니다.


```java
abstract class Animal {
    abstract void sound();
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍!");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("야옹!");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();

        makeSound(dog);
        makeSound(cat);
    }

    public static void makeSound(Animal animal) {
        animal.sound();
    }
}

```
이 예제에서는 `Animal` 추상 클래스를 사용하여 동물을 나타내고, `Dog`와 `Cat` 클래스를 구체적인 동물로 확장합니다. `Animal` 클래스의 `sound()` 메서드는 하위 클래스에서 구현되어야 하며, 각각의 동물에 맞는 소리를 출력하게 됩니다.

`makeSound()` 함수는 `Animal` 클래스를 매개변수로 받아서 동물의 소리를 출력합니다. 이 예제에서는 개방폐쇄원칙을 준수하고 있으며, 새로운 동물을 추가하려면 단순히 새로운 하위 클래스를 만들고 `sound()` 메서드를 구현하면 됩니다. 이렇게 함으로써 기존 코드를 수정하지 않고도 동물의 종류를 확장할 수 있습니다.