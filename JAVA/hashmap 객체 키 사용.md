java의 `HashMap`에서 사용자 정의 객체를 키로 사용하려면, 그 객체가 `equals()` 메서드와 `hashCode()` 메서드를 올바르게 구현해야 합니다. `HashMap`은 키의 동등성을 확인할 때 `equals()` 메서드를 사용하고, 키를 저장할 버킷을 결정할 때 `hashCode()` 메서드를 사용합니다.

### `equals()`와 `hashCode()` 구현

1. **`equals()` 메서드**: 두 객체가 같은지를 결정합니다. 이 메서드는 반사성(reflexive), 대칭성(symmetrical), 추이성(transitive), 일관성(consistent)의 특성을 가지고 있어야 합니다.
2. **`hashCode()` 메서드**: 객체의 해시 코드를 반환합니다. `equals()` 메서드가 두 객체를 같다고 판단하면, 이들 객체의 `hashCode()` 메서드는 같은 정수 값을 반환해야 합니다.


```java
import java.util.Objects;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    // getters and setters
}

```
이 클래스에서 `equals()` 메서드는 `name`과 `age` 필드가 모두 같은 경우에만 두 `Person` 객체를 같다고

판단하도록 구현되었습니다. `hashCode()` 메서드는 `name`과 `age` 필드를 기반으로 해시코드를 생성합니다.

### 주의사항

- **일관된 구현**: `equals()`와 `hashCode()`는 일관되게 구현되어야 합니다. 즉, `equals()` 메서드가 두 객체를 같다고 판단하면, `hashCode()` 메서드도 두 객체에 대해 동일한 정수 값을 반환해야 합니다.
- **불변 필드 사용**: `HashMap`의 키로 사용되는 객체의 필드가 변경되면, 해시코드 값이 변경될 수 있으므로, 키 객체는 가급적 불변성을 유지해야 합니다. 객체의 상태가 변경되면 해당 객체를 키로 사용하는 `HashMap`의 동작이 예측 불가능해질 수 있습니다.
- **성능 고려**: `hashCode()` 메서드는 해시 충돌을 최소화하면서도 빠르게 계산될 수 있도록 구현해야 합니다. 너무 복잡한 해시 계산은 성능 저하를 초래할 수 있습니다.

이러한 구현을 통해 사용자 정의 객체를 `HashMap`의 키로 안전하고 효율적으로 사용할 수 있습니다.