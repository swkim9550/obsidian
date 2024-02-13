인터페이스 분리 원칙(Interface Segregation Principle, ISP)은 "클라이언트는 자신이 사용하지 않는 메소드에 의존하면 안 된다"라는 원칙입니다. 즉, 하나의 범용 인터페이스보다는, 구체적인 여러 인터페이스가 낫다는 개념입니다. 이 원칙은 큰 인터페이스를 특정 클라이언트를 위한 더 작고 구체적인 인터페이스로 분리함으로써, 클라이언트가 자신과 무관한 인터페이스에 의존하지 않도록 합니다.


```java
interface Machine {
    void print(Document d);
    void scan(Document d);
    void fax(Document d);
}

class MultiFunctionPrinter implements Machine {
    public void print(Document d) {
        // ...
    }

    public void scan(Document d) {
        // ...
    }

    public void fax(Document d) {
        // ...
    }
}

class SimplePrinter implements Machine {
    public void print(Document d) {
        // ...
    }

    public void scan(Document d) {
        throw new UnsupportedOperationException();
    }

    public void fax(Document d) {
        throw new UnsupportedOperationException();
    }
}

```


```java
//interface segregation 을 잘지킨 예시
interface Printer {
    void print(Document d);
}

interface Scanner {
    void scan(Document d);
}

interface Fax {
    void fax(Document d);
}

class SimplePrinter implements Printer {
    public void print(Document d) {
        // ...
    }
}

class MultiFunctionMachine implements Printer, Scanner, Fax {
    public void print(Document d) {
        // ...
    }

    public void scan(Document d) {
        // ...
    }

    public void fax(Document d) {
        // ...
    }
}

```