## **점층적 생성자 패턴** **(Telescoping Constructor Pattern)**

- 필수 매개변수만 생성자부터, 필수 매개변수와 선택 매개변수 1개를 받는 생성자, 선택 매개변수 2개까지 받는 생성자, ... 형태로 선택 매개변수를 전부 받는 생성자까지 늘려가는 방식.
- 점층적 생성자 패턴 - 확장하기 어려움.
    
    ```java
    public class NutritionFacts{
        private final int servingSize;  // 필수
        private final int servings;     // 필수
        private final int calories;     // 선택
        private final int fat;          // 선택
        private final int sodium;       // 선택
        private final int carbohydrate; // 선택
    
        public NutritionFacts(int servingSize, int servings){
            this(servingSize, servings, 10);
        }
    
        public NutritionFacts(int servingSize, int servings, int calories){
            this(servingSize, servings, calories, 0);
        }
    
        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat){
            this(servingSize, servings, calories, fat, 0);
        }
    
        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat, int sodium){
            this(servingSize, servings, calories, fat, sodium, 0);
        }
    
        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat, int sodium, int carbohydrate){
            this.servingSize = servingSize;
            this.servings = servings;
            this.calories = calories;
            this.fat = fat;
            this.sodium = sodium;
            this.carbohydrate = carbohydrate;
        }
    }
    ```
    
    - NutritionFacts 클래스의 인스턴스를 만들기 위해 아래처럼 원하는 매개변수를 모두 포함한 생성자 중 가장 짧은 것을 골라 호출하면 됨.
        
        ```java
        NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
        ```
        
        - 이런 생성자는 사용자가 설정하길 원치 않는 매개변수까지 포함하기 쉬운데, 어쩔 수 없이 그런 매개변수에도 값을 지정해줘야 함.
        - 점층적 생성자 패턴도 쓸 수는 있지만, 매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어려움.
            
            

## **자바빈즈 패턴 (JavaBeans Pattern)**

- 이 패턴은 매개변수가 없는 생성자로 객체를 만든 후, setter 메서드들을 호출해 원하는 매개변수의 값을 설정하는 방식.
    
    ```java
    public class NutritionFacts {
    		// 매개변수들은 (기본값이 있다면) 기본값으로 초기화됨
        private int servingSize = -1;  // 필수, 기본값 없음
        private int servings = -1;     // 필수, 기본값 없음
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;
    
        public NutritionFacts() { }
    
        public void setServingSize(int val) { ... }
        public void setServings(int servings) { ... }
        public void setCalories(int calories) { ... }
        public void setFat(int fat) { ... }
        public void setSodium(int sodium) { ... }
        public void setCarbohydrate(int carbohydrate) { ... }
    
    }
    ```
    
    - 코드가 길어지긴 했지만 인스턴스를 만들기 쉽고, 그 결과 더 읽기 쉬운 코드가 됨.
    - 자바빈즈패턴에서는 객체 하나를 만들려면 메서드를 여러 개 호출해야 하고, 객체가 완전히 생성되기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 됨.
        - 점층적 생성자 패턴에서는 매개변수들이 유효한지를 생성자에서만 확인하면 일관성을 유지할 수 있었는데, 그 장치가 완전히 사라진 것.

## **빌더 패턴(Builder Pattern)**

- 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자(혹은 정적 팩터리)를 호출해 빌더 객체를 얻음.
- 그런 다음 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수들을 설정하고 매개변수가 없는 build 메서드를 호출해 원하는 객체를 얻음.
- 빌더는 생성할 클래스 안에 정적 멤버 클래스로 만들어두는 게 보통임.

```java
public static class Builder {
    // 필수 매개변수
    private final int servingSize;
    private final int servings;
 
    // 선택 매개변수 - 기본값으로 초기화
    private int calories = 0;
    private int fat = 0;
    private int sodium = 0;
    private int carbohydrate = 0;
 
    public Builder(int servingSize, int servings) {
      this.servingSize = servingSize;
      this.servings = servings;
    }
 
    public Builder calories(int val) {
      calories = val;
      return this;
    }
 
    public Builder fat(int val) {
      fat = val;
      return this;
    }
 
    public Builder sodium(int val) {
      sodium = val;
      return this;
    }
 
    public Builder carbohydrate(int val) {
      carbohydrate = val;
      return this;
    }
 
    public NutritionFacts build() {
      return new NutritionFacts(this);
    }
  }
 
  private NutritionFacts(Builder builder) {
    servingSize = builder.servingSize;
    servings = builder.servings;
    calories = builder.calories;
    fat = builder.fat;
    sodium = builder.sodium;
    carbohydrate = builder.carbohydrate;
  }
}
```

- NutritionFacts 클래스는 불변이며, 모든 매개변수의 기본값들을 한 곳에 모아둠.
- 이 클래스를 사용한 클라이언트 코드
    
    ```java
    NutritionFacts cocaCola = new NutritionFacts.Builder(240, 9)
                .calories(100).sodium(35).carbohydrate(27).build();
    ```
    
- 빌더는 계층적으로 설계된 클래스와 함께 쓰기에 좋음.
- 생성자로는 누릴 수 없는 이점
    - 빌더를 이용하면 가변인수 매개변수를 여러 개 사용하거나 메서드를 여러 번 호출하도록 하고 각 호출 때 넘겨진 매개변수들을 하나의 필드로 모을 수 있음.
- 빌더 하나로 여러 객체를 순회하면서 만들 수 있고, 빌더에 넘기는 매개변수에 따라 다른 객체를 만들 수도 있음.
- Builder 를 항상 생성해야된다는 단점이 있음.
    - 성능에 민감한 상황에서는 문제가 될 수 있음.
