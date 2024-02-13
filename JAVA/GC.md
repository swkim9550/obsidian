
Java의 Garbage Collection (GC)는 메모리 관리를 위한 중요한 부분입니다. GC는 더 이상 사용되지 않는 객체를 식별하고 해제하여 프로그램의 메모리를 관리합니다. 아래에서는 Java GC가 발생하는 과정을 자세히 설명합니다.

### 메모리영역 

1. **Eden 영역**:
    
    - Eden 영역은 새로 생성된 객체가 할당되는 초기 메모리 영역입니다.
    - 새로운 객체는 먼저 Eden 영역에 할당되고, GC가 실행될 때까지 이곳에서 유지됩니다.
    - Eden 영역이 가득 차면 새로운 객체 중 일부는 Survivor 영역으로 이동하고, 나머지는 삭제됩니다.
2. **Survivor 영역 (S0와 S1)**:
    
    - Survivor 영역은 Eden 영역에서 일정 기간 동안 살아남은 객체들이 이동하는 임시 영역입니다.
    - 일반적으로 두 개의 Survivor 영역이 있으며, S0와 S1로 표시됩니다.
    - GC 실행 시, 살아남은 객체는 한 Survivor 영역에서 다른 Survivor 영역으로 이동합니다. 이러한 이동은 객체의 수명을 추적하고 메모리 누수를 방지하는 데 도움이 됩니다.
3. **Old 영역 (또는 Tenured 영역)**:
    
    - Old 영역은 Eden과 Survivor 영역에서 오래 살아남은 객체들이 최종적으로 이동하는 영역입니다.
    - Old 영역은 큰 크기의 객체 및 오랜 기간 동안 살아있는 객체를 보관합니다.
    - Old 영역에 있는 객체는 일반적으로 GC의 대상이 되지 않으며, 오랜 시간 동안 메모리를 점유하므로 주기적인 GC 실행이 필요합니다.
4. **Perm 또는 Metaspace 영역**:
    
    - Perm(Permanent) 영역은 Java 7 이전의 JVM에서 사용되었으며, 클래스 메타데이터 및 상수 풀 등을 저장하는 영역이었습니다.
    - Java 8부터는 Perm 영역이 Metaspace 영역으로 대체되었습니다.
    - Metaspace 영역은 클래스 로딩 및 메타데이터 저장을 담당하며, 필요한 만큼 동적으로 크기를 조정할 수 있습니다.


### GC 종류(마이너,메이저 (full) GC)

**마이너(Minor) GC:** 마이너 GC는 주로 Young Generation에서 발생하는 GC입니다. Young Generation은 새로 생성된 객체들이 할당되는 영역으로, 대부분의 객체는 여기서 생성되며 짧은 수명을 가집니다. 마이너 GC는 주로 다음과 같은 상황에서 발생합니다:

1. **Eden 영역이 가득 찰 때**: Eden 영역이 새로 생성된 객체로 가득 차면, 마이너 GC가 실행되어 Eden 영역에서 더 이상 사용되지 않는 객체를 제거하고, 살아남은 객체를 Survivor 영역으로 이동시킵니다.
    
2. **Survivor 영역이 가득 찰 때**: Survivor 영역은 살아남은 객체가 일정 기간 유지되는 임시 영역입니다. 두 개의 Survivor 영역(S0와 S1)이 있고, GC가 실행될 때마다 이동이 발생합니다. 살아남은 객체는 한 Survivor 영역에서 다른 Survivor 영역으로 이동됩니다.
    

마이너 GC는 주로 저비용으로 실행되며, Young Generation에서만 수행되므로 전체 GC 시간에 비해 빠릅니다. 대부분의 객체가 Young Generation에서 죽기 때문에 빈번하게 발생하지만, Old Generation에 있는 객체는 건드리지 않습니다.


**풀(Full) GC:** 풀 GC는 주로 Old Generation에서 발생하는 GC로, Old Generation 영역이 가득 차거나 기타 상황에서 발생합니다. 풀 GC는 Young Generation과 Old Generation 모두를 대상으로 하며, 전체 메모리를 스캔하여 더 이상 참조되지 않는 객체를 모두 제거합니다.

풀 GC는 주로 다음과 같은 상황에서 발생합니다:

1. **Old Generation이 가득 찰 때**: Old Generation 영역에는 오래된 객체가 존재하므로, Old Generation이 가득 찰 경우 풀 GC가 실행되어 모든 객체를 검사하고 쓰레기를 수집합니다.
    
2. **Perm 또는 Metaspace 영역이 가득 찰 때**: 클래스 메타데이터 및 상수 풀 등을 저장하는 Perm 또는 Metaspace 영역이 가득 찰 경우, 해당 영역의 GC가 실행됩니다.
    

풀 GC는 상대적으로 비용이 높으며 실행 시간도 오래 걸릴 수 있습니다. 따라서 풀 GC는 최대한 피하는 것이 좋으며, 메모리 누수를 방지하고 Young Generation GC가 자주 발생하지 않도록 메모리 관리를 최적화하는 것이 중요합니다.



### Stop-the-World

"Stop-the-World"는 Java 가상 머신 (JVM)의 Garbage Collection (GC) 작업 중 하나로, 애플리케이션의 모든 스레드가 일시적으로 멈추는 현상을 의미합니다. 이러한 멈춤은 GC가 메모리 관리를 위해 실행되는 동안 발생하며, 모든 애플리케이션 스레드가 일시적으로 중단되어야 합니다.

Stop-the-World 현상이 발생하는 이유는 다음과 같습니다:

1. **GC 수행**: GC는 불필요한 객체를 식별하고 메모리를 회수하기 위해 수행됩니다. 이 과정에서 모든 스레드가 일시적으로 멈춰야 합니다.
    
2. **메모리 정리**: Stop-the-World 시간 동안 GC는 메모리 정리 및 객체 삭제 작업을 수행합니다. 이 작업은 메모리 누수를 방지하고 메모리 사용량을 최적화합니다.
    
3. **애플리케이션 영향**: Stop-the-World 현상은 애플리케이션 성능에 영향을 미칠 수 있습니다. 특히 큰 메모리를 사용하는 애플리케이션에서는 긴 Stop-the-World 시간이 발생하면 애플리케이션의 응답성이 떨어질 수 있습니다.
    

다양한 GC 알고리즘과 JVM 구현에서는 Stop-the-World 현상을 최소화하려는 노력이 있습니다. 예를 들어, CMS (Concurrent Mark-Sweep) GC 및 G1 (Garbage-First) GC와 같은 알고리즘은 일부 작업을 병행하거나 (concurrent) 일부 영역만 멈추는 방식으로 Stop-the-World 시간을 최소화합니다. 하지만 어쨌든 GC 작업 도중에는 Stop-the-World 현상이 발생하며, 이를 최적화하고 이해하는 것이 중요합니다.


### GC 튜닝
Java Garbage Collection (GC) 튜닝은 애플리케이션의 성능과 메모리 사용량을 최적화하는 중요한 작업입니다. GC 튜닝을 통해 애플리케이션의 중단 시간을 최소화하고 메모리 누수를 방지할 수 있습니다. 아래는 Java GC 튜닝을 위한 일반적인 방법과 고려해야 할 사항입니다:

1. **GC 알고리즘 선택**:
    
    - JVM은 다양한 GC 알고리즘을 제공합니다. 애플리케이션의 특성에 따라 적합한 GC 알고리즘을 선택해야 합니다.
    - Young Generation에서는 G1 GC 또는 CMS GC와 같은 병행 GC 알고리즘을 사용할 수 있으며, Old Generation에서는 G1 GC 또는 Serial GC를 고려할 수 있습니다.
2. **힙 크기 조정**:
    
    - 힙 크기를 적절하게 조정하여 메모리 사용량과 GC 작업에 영향을 미칠 수 있습니다. 힙 크기를 늘리면 GC 주기가 늘어나지만, 메모리 소비가 늘어납니다.
    - `-Xms`와 `-Xmx` 옵션을 사용하여 초기 힙 크기와 최대 힙 크기를 설정할 수 있습니다.
3. **Young Generation 비율 조정**:
    
    - Young Generation 영역의 크기를 조정하여 GC 주기와 중단 시간을 관리할 수 있습니다. 큰 Young Generation은 더 빈번한 마이너 GC를 유발하지만 중단 시간을 줄일 수 있습니다.
    - `-XX:NewRatio` 및 `-XX:NewSize` 옵션을 사용하여 Young Generation 크기를 조절할 수 있습니다.
4. **Survivor 영역 비율 조정**:
    
    - Survivor 영역의 크기도 조절 가능합니다. 작은 Survivor 영역은 객체가 더 빨리 Old Generation으로 이동하도록 할 수 있습니다.
    - `-XX:SurvivorRatio` 옵션을 사용하여 Survivor 영역 크기를 조절할 수 있습니다.
5. **GC 로깅 및 모니터링**:
    
    - GC 로그를 활성화하고 모니터링 도구를 사용하여 GC 동작을 실시간으로 추적하고 문제를 진단할 수 있습니다.
    - `-XX:+PrintGC` 및 `-XX:+PrintGCDetails` 옵션을 사용하여 GC 로그를 활성화할 수 있습니다.
6. **GC 튜닝 도구 사용**:
    
    - 다양한 GC 튜닝 도구와 프로파일러를 활용하여 메모리 누수 및 성능 문제를 식별하고 해결할 수 있습니다.
    - 예를 들어, VisualVM, jstat, jconsole 등의 도구를 사용할 수 있습니다.
7. **코드 리뷰 및 최적화**:
    
    - GC 튜닝은 메모리 누수를 방지하고 불필요한 객체 생성을 최소화하는 것과도 관련이 있습니다. 코드 리뷰를 통해 효율적인 객체 사용 및 메모리 관리를 확인할 수 있습니다.
8. **GC 이벤트 핸들링**:
    
    - 애플리케이션 코드에서 GC 이벤트를 핸들링하고 적절한 대응을 설정할 수 있습니다. 예를 들어, GC 이벤트에 따라 특정 작업을 수행하거나 로그를 기록할 수 있습니다.

GC 튜닝은 애플리케이션의 특성에 따라 다르며, 여러 번의 실험과 모니터링을 통해 최적의 설정을 찾아야 합니다. GC 튜닝은 애플리케이션의 성능을 향상시키고 안정성을 확보하는 데 중요한 역할을 합니다.


### default GC

#### java8 [default parallel gc]
  
Java 8의 기본 GC 알고리즘은 Parallel GC (Parallel Garbage Collector)입니다. Java 8에서는 G1 GC(Garbage-First Garbage Collector)가 도입되었지만, Parallel GC가 여전히 기본 Young Generation GC 알고리즘으로 설정되어 있습니다.

Parallel GC는 멀티스레드 환경에서 Young Generation을 효율적으로 관리하며, 빠른 마이너 GC를 제공하는 알고리즘입니다. Parallel GC는 `-XX:+UseParallelGC` 옵션을 통해 활성화할 수 있습니다.

G1 GC는 Java 8에서 도입되었지만, 기본적으로는 Parallel GC를 사용하는 것으로 설정되어 있습니다. G1 GC를 기본 Young Generation GC 알고리즘으로 사용하려면 `-XX:+UseG1GC` 옵션을 명시적으로 지정해야 합니다. G1 GC는 메모리 사용량을 조절하고 중단 시간을 최소화하기 위한 현대적인 GC 알고리즘으로 Java 8에서 많은 개선 사항을 제공합니다.

#### java9 [default g1gc]

Java 9에서는 기본 GC 알고리즘이 변경되었습니다. Java 9에서의 기본 GC 알고리즘은 G1 GC (Garbage-First Garbage Collector)입니다. 이전 버전에서는 Parallel GC가 기본 Young Generation GC 알고리즘이었지만, Java 9부터는 G1 GC가 Young Generation GC 알고리즘으로 설정되었습니다.

G1 GC는 메모리 사용량을 효율적으로 관리하고 중단 시간을 최소화하기 위한 현대적인 GC 알고리즘으로, Java 9부터는 기본적으로 사용되며 별도의 옵션 설정이 필요하지 않습니다. G1 GC는 대규모 힙 및 다중 프로세서 시스템에서 성능을 최적화하도록 설계되었습니다.


### GC 종류

#### G1GC

G1 GC (Garbage-First Garbage Collector)는 Java의 Garbage Collection (GC) 알고리즘 중 하나로, 대규모 힙 메모리에서 중단 시간을 최소화하면서 메모리를 효율적으로 관리하기 위해 설계된 알고리즘입니다. G1 GC는 Young Generation과 Old Generation을 나누고 관리하는 동시성 GC 알고리즘입니다.

- 힙을 여러개의 영역으로 나누고 관리 -> region으로 관리한다라고 표현함
	- region은 G1HeapResionSize 옵션을 통해서 영역을 설정할 수 있음(2048개의 Region으로 1MB ~32MB까지 지정 가능)
- Eden, Survivor, Old Generation, Humongous, available/unused 영역 등
- Humongous 영역은 큰 객체를 저장하고 처리하기 위한 영역
	- Region 크기의 50%를 초과하는 큰 객체를 저장하기 위한 공간
- available/unused
	- 아직 사용되지 않은 Region 영역
- 중단 시간 최소화하고 메모리 관리를 효율적으로 하는게 목적
- Mixed GC 발생 시 old generation의 부분적인 가비지 수집이 발생한다.  일부 Humongous 객체도 함께 처리 된다.
	- young/old 영역을 모두 GC 하는 것을 의미함.
- 가비지가 많을 것으로 예상되는 영역에 대한 수집 및 압축이 주가된다 -> Garbage Frist
- 결론적으로 stop-the-world 지향하는 알고리즘 이며 CMS 알고리즘의 개선안으로 계획됨

