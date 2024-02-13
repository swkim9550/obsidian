멀티스레딩 환경에서 두 개 이상의 스레드가 서로의 작업이 끝나기를 무한히 기다리는 상태를 말합니다. 이로 인해 해당 스레드들은 영원히 진행되지 못하고, 자원을 사용할 수 없게 되어 프로그램이 멈추게 됩니다

1. **상호 배제(Mutual Exclusion)**: 자원은 한 번에 하나의 스레드만 사용할 수 있습니다. 즉, 다른 스레드가 해당 자원을 사용하고 있다면, 다른 스레드는 그 자원이 해제될 때까지 기다려야 합니다.
    
2. **보유 및 대기(Hold and Wait)**: 스레드가 현재 어떤 자원을 보유하고 있는 상태에서 다른 추가 자원을 기다립니다.
    
3. **비선점(No Preemption)**: 자원은 스레드에 의해 점유되면, 그 스레드가 자발적으로 자원을 해제할 때까지 다른 스레드가 그 자원을 빼앗을 수 없습니다.
    
4. **순환 대기(Circular Wait)**: 각 스레드는 순환적으로 다음 스레드가 요구하는 자원을 보유하고 있어, 서로가 서로의 자원을 기다리는 상황이 발생합니다.
    

교착상태를 해결하거나 예방하기 위한 일반적인 방법은 다음과 같습니다:

1. **자원 할당 순서 정하기**: 모든 스레드가 자원을 같은 순서로 요청하도록 하여 순환 대기 조건을 방지합니다.
    
2. **자원을 한 번에 할당하기**: 스레드가 실행되기 전에 필요한 모든 자원을 한 번에 할당받도록 하여 보유 및 대기 조건을 방지합니다.
    
3. **비선점 자원 사용 제한**: 가능하다면 자원을 선점할 수 있게 하여 한 스레드가 다른 스레드의 자원을 빼앗을 수 있도록 합니다.
    
4. **자원 요청 시 대기 시간 설정**: 스레드가 자원을 기다리는 시간에 제한을 두어, 일정 시간이 지나면 자원 요청을 포기하고 다시 시도하게 합니다.


## JAVA example
___
```java
public class DeadlockExample {
    public static void main(String[] args) {
        // 두 개의 자원
        final Object resource1 = "resource1";
        final Object resource2 = "resource2";
        
        // 첫 번째 스레드는 resource1에 락을 건 후 resource2에 락을 걸려고 시도
        Thread t1 = new Thread() {
            public void run() {
                synchronized (resource1) {
                    System.out.println("Thread 1: Locked resource 1");
                    
                    try { Thread.sleep(100);} catch (Exception e) {}
                    
                    synchronized (resource2) {
                        System.out.println("Thread 1: Locked resource 2");
                    }
                }
            }
        };

        // 두 번째 스레드는 resource2에 락을 건 후 resource1에 락을 걸려고 시도
        Thread t2 = new Thread() {
            public void run() {
                synchronized (resource2) {
                    System.out.println("Thread 2: Locked resource 2");
                    
                    try { Thread.sleep(100);} catch (Exception e) {}
                    
                    synchronized (resource1) {
                        System.out.println("Thread 2: Locked resource 1");
                    }
                }
            }
        };
        
        t1.start();
        t2.start();
    }
}

```