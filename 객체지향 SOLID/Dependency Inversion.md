  
의존관계 역전 원칙(Dependency Inversion Principle, DIP)은 고수준 모듈이 저수준 모듈에 의존하지 않도록 하고, 둘 다 추상화에 의존하도록 하는 원칙입니다. 추상화는 구체적인 세부 사항에 의존하지 않아야 하며, 세부 사항은 추상화에 의존해야 합니다. 이 원칙은 시스템의 결합도를 낮추고, 유연성과 확장성을 증가시키는 데 도움이 됩니다.


```java
class LightBulb {
    public void turnOn() {
        System.out.println("전구를 켭니다.");
    }

    public void turnOff() {
        System.out.println("전구를 끕니다.");
    }
}

class Switch {
    private LightBulb bulb;

    public Switch() {
        this.bulb = new LightBulb();
    }

    public void operate() {
        if (bulb.isOn()) {
            bulb.turnOff();
        } else {
            bulb.turnOn();
        }
    }
}


```

위 코드에서는 `Switch` 클래스가 `LightBulb` 클래스에 직접 의존하고 있으며, `Switch` 클래스 내에서 `LightBulb` 객체를 직접 생성하고 사용합니다. 이것은 DIP를 준수하지 않는 예시입니다.


```java
// 추상화된 인터페이스
interface Switchable {
    void turnOn();
    void turnOff();
    boolean isOn();
}

class LightBulb implements Switchable {
    private boolean on;

    @Override
    public void turnOn() {
        System.out.println("전구를 켭니다.");
        on = true;
    }

    @Override
    public void turnOff() {
        System.out.println("전구를 끕니다.");
        on = false;
    }

    @Override
    public boolean isOn() {
        return on;
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        if (device.isOn()) {
            device.turnOff();
        } else {
            device.turnOn();
        }
    }
}


```
위 코드에서는 DIP를 준수하기 위해 `Switch` 클래스가 `Switchable` 인터페이스를 사용하고 있으며, `LightBulb` 클래스도 `Switchable` 인터페이스를 구현하고 있습니다. `Switch` 클래스는 `Switchable` 인터페이스를 주입받아서 다양한 장치를 다룰 수 있게 되었습니다. 이렇게 하면 고수준 모듈(`Switch`)과 저수준 모듈(`LightBulb`) 모두 추상화에 의존하게 되며, DIP를 준수합니다.

```java
// 추상화된 인터페이스
interface Animal {
    void feed();
    void groom();
}

// 저수준 모듈: 개
class Dog implements Animal {
    public void feed() {
        System.out.println("Dog is being fed.");
    }

    public void groom() {
        System.out.println("Dog is being groomed.");
    }
}

// 저수준 모듈: 고양이
class Cat implements Animal {
    public void feed() {
        System.out.println("Cat is being fed.");
    }

    public void groom() {
        System.out.println("Cat is being groomed.");
    }
}

// 고수준 모듈: 동물 케어테이커
class AnimalCaretaker {
    private Animal animal;
    
    public AnimalCaretaker(Animal animal) {
        this.animal = animal;
    }
    
    public void takeCare() {
        animal.feed();
        animal.groom();
    }
}

public class Main {
    public static void main(String[] args) {
        Animal dog = new Dog();
        AnimalCaretaker caretakerForDog = new AnimalCaretaker(dog);
        caretakerForDog.takeCare(); // Dog is being fed. Dog is being groomed.

        Animal cat = new Cat();
        AnimalCaretaker caretakerForCat = new AnimalCaretaker(cat);
        caretakerForCat.takeCare(); // Cat is being fed. Cat is being groomed.
    }
}

```