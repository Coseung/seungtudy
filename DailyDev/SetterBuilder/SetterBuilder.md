## 들어가기 
`Spring boot` 를 공부하고 프로젝트를 간단히 만들다 보면 `Getter` `Setter`를 써보기 시작합니다 생성자 보다 간단하고 너무 편리해서 작성하는데 점점 공부를 할수록 'Setter' 는 쓰면 좋지않다. `Builder` 를 써라 와 같은 소리가 하나 둘 씩 보여서 정리 해보았습니다. 

## 일반적인 생성자 
일반적인 생성자로 개체를 생성하면 어떤 문제점이있을까요? 

가장 기본적인 방법입니다. 필수 인자부터 선택 인자까지 생성자를 계속 오버로딩해서 만드는 방식이죠.

```java
public class Pizza {
    private String dough;   
    private String source;   
    private int cheese;      
    private int bacon;       
    private int pineapple;   

    // 1. 필수 인자만 받는 생성자
    public Pizza(String dough, String source) {
        this(dough, source, 0, 0, 0);
    }

    // 2. 치즈가 추가된 생성자
    public Pizza(String dough, String source, int cheese) {
        this(dough, source, cheese, 0, 0);
    }

    // 3. 치즈, 베이컨이 추가된 생성자
    public Pizza(String dough, String source, int cheese, int bacon) {
        this(dough, source, cheese, bacon, 0);
    }

    // 4. 모든 재료를 받는 생성자
    public Pizza(String dough, String source, int cheese, int bacon, int pineapple) {
        this.dough = dough;
        this.source = source;
        this.cheese = cheese;
        this.bacon = bacon;
        this.pineapple = pineapple;
    }
}
```

### 뭐가 문제일까요?
선택 인자가 늘어날수록 생성자를 계속 만들어야 하는 상황이 펼쳐집니다. 사용하는 입장에서도 문제입니다.


```java
Pizza pizza = new Pizza("Thin", "Tomato", 1, 0, 0);
```
도대체 1, 0, 0이 무슨 의미일까요? 순서라도 헷갈리면 도우에 Tomato가 들어갈수도있고 그러겠죠? 


---
## Setter 로 해결해보기 
생성자가 너무 복잡하니, "일단 객체를 만들고 값을 나중에 채워 넣자!" 느낌으로 사용하는것이 바로 `Setter`를 사용하는 방식입니다.


```java
Pizza myPizza = new Pizza();
myPizza.setDough("Thin");
myPizza.setSource("Tomato");
myPizza.setCheese(2); 
```
베이컨, 파인애플은 안 먹고 싶으면 안 부르면 끝인것입니다. 아주 편리하지않나요? 


---
## 하지만 🫸
하지만... 치명적인 단점이 있습니다
편리함 뒤에 숨겨진 문제라고 할수있죠

### 1. 객체의 일관성이 깨집니다.
``Setter``는 단순히 값을 할당만 하기 때문에, 객체 내부의 검증 로직을 우회할 위험이 있습니다.

예를 들어, 피자 도우를 변경할 때 유효한 값인지 확인하는 `changeDough()` 메서드를 만들었다고 가정해 봅시다.


```java
public class Pizza {
    private String dough;

    // 도우 변경 시 유효성 검사를 수행하는 메서드 changeDough
    public void changeDough(String newDough) {
        if (!newDough.equals("Thin") && !newDough.equals("Thick")) {
            throw new IllegalArgumentException("유효하지 않은 도우 타입입니다.");
        }
        this.dough = newDough;
    }

    // 단순히 값을 변경하는 Setter
    public void setDough(String dough) {
        this.dough = dough;
    }
}
```
문제점: 검증 로직이 무시됨 개발자가 `changeDough()` 대신 습관적으로 `setDough()`를 사용하면, 검증 과정 없이 값이 변경됩니다.



```java
myPizza.setDough("Rubber"); // 유효하지 않은 값("Rubber")이 그대로 설정됨
```



### 2. 객체의 불변성을 만들 수 없습니다. 
Setter가 있다는 것은 **"언제든지 값을 바꿀 수 있다"**는 뜻입니다.

```java
// 피자가 완성돼서 배달 중인 상황
myPizza.setCheese(0); // 누군가 중간에 치즈를 없애버릴 수 있음!
```
객체가 안전하게 보호받으려면 한 번 생성된 후에는 변하지 않아야 하는데, `Setter`는 그냥 열어둡니다. 배달된 피자도 누군가 먹고 벽돌로 채워놓을 수도있는것이죠. 

---
## 빌더로 문제를 해결한다면 
생성자의 안정성과 `Setter`의 가독성을 모두 잡은 것이 바로 빌더 패턴입니다. `Lombok` 라이브러리를 사용해 아주 간단하게 적용하여 예시를 들어보겠습니다.


```java
import lombok.Builder;
import lombok.Getter;
import lombok.ToString;

@Getter           
@ToString         
@Builder         
public class Pizza {
    
    private final String dough;
    private final String source;
    
    @Builder.Default 
    private int cheese = 0;
    
    @Builder.Default
    private int bacon = 0;
    
    @Builder.Default
    private int pineapple = 0;
}
```
사용하는 모습

```java
Pizza bestPizza = Pizza.builder()
                .dough("Thin")
                .source("Tomato") 
                .cheese(2)       
                .bacon(10)                       
                .build();         

```
가독성도 높아지고, 순서도 안맞춰도 되고 build를 하는 시점에서 객체가 생성되며 값을 넣어줍니다.

---
## 그래서 Setter보다 뭐가 좋은건데?
물론 `Setter`도 직관적이고 편하다는 장점이 있습니다. 하지만 협업과 유지보수 관점에서 빌더 패턴은 `Setter`의 장점을 커버하면서 더 큰 안정성을 제공합니다.

#### 1. 객체의 필드가 많을 때  유리합니다
주문 DTO를 받아서 Entity로 변환하는 과정을 예로 들어보겠습니다.

[Setter 사용 시]

```java
public Pizza order(OrderDto dto) {
    Pizza pizza = new Pizza();
    pizza.setDough(dto.getDough());
    pizza.setSource(dto.getSource());
    pizza.setCheese(dto.getCheese());
    // ... 토핑이 20개라면 20줄을 더 써야 함
    return pizza;
}
```
한 줄 한 줄 매핑해야 해서 코드가 길어지고, 실수로 필드 하나를 빼먹어도 컴파일러가 잡아주지 못합니다.

[Builder 사용 시]

```java
public Pizza order(OrderDto dto) {
    return Pizza.builder()
            .dough(dto.getDough())
            .source(dto.getSource())
            .cheese(dto.getCheese())
            .build();
}
```
변수명을 반복해서 쓸 필요가 없고, 어떤 필드에 어떤 값이 들어가는지 한눈에 그룹화되어 보이지않나요?

#### 2. "생성"인지 "수정"인지 명확
가장 중요한 포인트입니다. 코드가 복잡해졌을 때 Setter는 의도를 파악하기 어렵게 만듭니다.

[Setter: 의도가 불분명함]

```java
public void updateLogic(PizzaDto dto) {
    // 이게 초기화 과정인지, 나중에 수정을 하는 건지 알기 어려움
    pizza.setDough(dto.getDough()); 
}
```
빌더 패턴을 사용하면 보통 `Setter`를 닫아둡니다. 대신 생성은 `builder()`, 수정은 `changeDough()` 같은 커스텀 메서드를 사용하게 됩니다.
```java
public void updateLogic(PizzaDto dto) {
    // 도우변경(의도가 명확함)
    pizza.changeDough(dto.getDough());
}
```

#### 3. 수정을 막습니다
`setDough()`는 생성, 수정, 단순 변환 등 모든 곳에서 쓰일 수 있어 통제가 불가능합니다. 반면 `changeDough()`는 오직 **'수정'**이라는 목적이 있을 때만 호출합니다.




---
## 결론
혼자 하는 프로젝트에서는 빠르게 개발해야 하고 구조를 내가 다 아니까 Setter가 더 효율적일 수 있다고 생각이 듭니다.

팀 프로젝트에서는 이 코드가 수정을 하는 건지, 생성을 하는 건지 헷갈리기 시작하면 팀원들이 유지보수할때 힘들지 않을까요? 

>뭐 뒤도 안돌아보고 돌아가기만 하는 코드를 짠다면 상관없을듯 

버그가 발생했을 때 Setter가 여기저기 있으면, 데이터가 어디서 에러가 나는지 찾기 힘들기때문에 가독성, 명확한 의도 전달, 그리고 안전한 협업을 위해서 저는 Builder 패턴을 추천드립니다.

