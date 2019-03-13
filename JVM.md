### Stack, Heap 메모리 차이점?

- Stack : 스레드가 독립적으로 가지고 있는 영역, 메소드의 호출 정보, 지역변수 정보 보유

  Stack 영역은 독립적이기 때문에 각각의 메소드가 독립적으로 호출 가능

  - PC Register, Native Method Stack도 스레드가 독립적으로 보유

- Heap : 스레드가 공통적으로 사용하고 있는 공유 영역, 참조 객체 정보 보유

  - Method Area는 스레드가 공유해서 사용

    - 클래스로더가 바이트코드를 처음으로 적재하는 공간으로서, 클래스와 관한 모든 정보들을 보유 —> 리플랙션 가능 이유

      

### Multi Thread에서 Single 인스턴스를 사용할 때 주의할 점?

- 멀티 스레드에서 공유자원에 대해 접근했을 때 문제 발생 가능



### 자바코드 실행 과정 (컴파일 과정)

1. 컴파일러가 .java 파일을 .class 파일(바이트 코드)로 변환 (컴파일러 방식)

2. 클래스로더가 바이트코드를 Runtime Data Area에 적재

3. 컴파일러가 Runtime Data area에 적재된 바이트 코드를 해석하면서 실행 (인터프리터 방식)

   

### GC 

- Minor GC

  - Young area(Eden, Survivor1, 2 영역)에서의 GC를 의미
    1. 처음 생성된 객체는 Eden 영역에서 관리
    2. Eden 영역이 가득 찼을 때, GC 발생
    3. 참조되어 있는 Reachable 객체를 S1 영역으로 이동시키고, 해당 객체 age++, 참조되지 않는 객체 제거
    4. Eden 영역이 가득 찼을 때, GC 발생
    5. 참조되어 있는 객체를 S2 영역으로 이동시키고, 해당 객체 age++, 참조되지 않는 객체 제거
       S1 영역에서도 참조되지 않는 객체는 S2 이동시키고, 해당 객체 age++, 참조되지 않는 객체 제거
    6. 객체의 age가 특정 범위가 되었을 때, Old Area 이동
  - Table Card에는 Old Area에 있는 객체가 Young area의 객체를 참조할 때, 참조하고 있는 객체에 대한 정보 보유
    Table Card에 참조 관련된 정보가 있기 때문에 Minor GC에서 Old Area가 객체를 참조하는지 확인하는 작업 시간 단축 가능

- Major GC
  - Old area에서의 GC를 의미 (Mark, Sweep, Compact)
    1. 참조되는 객체를 식별 (Mark), 참조되지 않는 객체 식별 (Sweep), 참조되지 않는 객체를 제거하고, 
       한 곳으로 모은다 (Compact)

- GC의 성능튜닝은 Stop the world 시간의 단축을 의미

  - GC가 동작하는 과정에서 GC 수행하는 스레드를 제외하고 다른 스레드는 모두 정지하기 때문에 시간 단축 필요

  

### GC 대상

- 참조가 null 인 경우
- 객체가 블럭 안에서 생성되고 블럭이 종료된 경우
-  부모 객체가 null이 된 경우, 자식 객체는 자동적으로 GC 대상
- 객체가 Weak 참조만 가지고 있을 경우
- 객체가 Soft 참조이지만 메모리 부족이 발생한 경우



### Weak Referece, Strong Reference, Soft Reference



### Iterator, Stream 비교

- 결론적으로 Stream이 속도, 메모리 측면에서 효율이 낮음

  - Stream은 for each를 통해 반복할 수 록 Immutable한 객체를 계속 생성하기 때문에 메모리 저하
  - for loop는 jit compiler에 최적화되었기 때문에 속도 측면에서 빠름

  

### JIT Compiler

- 인터프리터 방식, 컴파일러 방식을 모두 사용하는 방식
  - 인터프리터 방식 : 소스코드 호출 시, .class 파일을 기계어로 번역
  - 컴파일러 방식 : JVM에 기계어가 아닌 JVM이 이해할 수 있는 .class(jar) 파일로 컴파일
- 인터프리터 속도 단점을 보완
  - 같은 코드를 매번 해석하지 않고 실행할 때 컴파일을 하면서 해당 코드를 캐싱해버립니다. 이후엔, 바뀐 부분만 컴파일 하고 나머지는 캐싱된 코드를 사용 하는 거죠. So, 인터프리터의 속도를 개선할 수 있습니다.

출처 : http://younghwakim.blogspot.com/2013/08/d1-2013-08-01_1.html
출처 : https://medium.com/@ahn428/java-jit-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC-c7d068e29f45
출처 : https://itmining.tistory.com/24
출처 : https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2



### Explain

- Unique vs Primary
  - Unique Key : NULL 가능 (단, NULL은 오직 한개만 가능), Foreign Key 설정 불가
  - Primary Key : NULL 불가, Foreign Key 설정 가능