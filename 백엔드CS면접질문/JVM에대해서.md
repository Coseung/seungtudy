## JVM에 대해 설명해주세요 
> JVM은 자바 가상머신으로 Java 바이트 코드를 OS에 맞게 해석해주는 역할을 합니다. 또, 가비지 컬렉션(GC)으로 자동적인 메모리 관리를 해줍니다.

---
# 꼬리질문

## JVM구조와 실행 방식은 어떻게 되나요? 

>JVM은 Class Loadeer와 Execution Engine, GC, 그리고 Runtime Data Area 가 있습니다.

Class Loader : JVM 내(Runtime Data Area)로 Class 파일을 로드하고 링크

Execution Engine : 메모리(Runtime Data Area)에 적재된 클래스들을 기계어로 변경해 실행

Garbage Collector : 힙 메모리에서 참조되지 않는 개체들 제거

Runtime Data Area : 자바 프로그램을 실행할 때, 데이터를 저장

#### 실행방식
> 자바 컴파일러가 소스코드를 바이트코드로 변환하면, 클래스 로더가 이를 JVM의 메모리 영역에 적재하고 실행엔진이 기계어로 해석하여 프로그램을 실행합니다. 

컴파일 (Compile): 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일합니다. (프로그램 실행 전 단계)

JVM 구동 및 메모리 할당: 자바로 개발된 프로그램을 실행하면 JVM이 구동되며, JVM은 OS로부터 프로그램 실행에 필요한 메모리(Runtime Data Area)를 할당받습니다.

클래스 로딩 (Class Loading): Class Loader(클래스 로더)를 통해 바이트코드(.class 파일들)를 JVM의 메모리 영역인 Runtime Data Area(구체적으로는 Method Area)로 로딩합니다.

실행 (Execution): Runtime Data Area에 로딩된 바이트코드는 Execution Engine(실행 엔진)을 통해 기계어로 해석되어 실제 CPU에서 실행됩니다. 이 과정에서 인터프리터(Interpreter)와 JIT(Just-In-Time) 컴파일러가 함께 사용됩니다.

메모리 및 런타임 관리: 프로그램이 실행되는 동안 발생하는 데이터(객체, 변수 등)는 Runtime Data Area의 각 영역(Heap, Stack 등)에 저장되어 사용됩니다. 이 실행 과정에서 JVM에 의해 Garbage Collection(GC)이 작동하여 사용하지 않는 메모리를 회수하고, 스레드 동기화 등의 작업이 이루어집니다

## GC가 뭔가요
>GC는 힙 영역에서 사용하지않은 객체들을 자동으로 제거해주는 시스템입니다. 


