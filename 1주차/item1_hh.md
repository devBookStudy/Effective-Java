- 클래스는 생성자와 별도로 정적 팩터리 메서드(static factory method)를 제공할 수 있음.
    - 정적 팩터리 메서드는 클래스에 정적 메서드를 정의하여, 생성자 대신 객체를 생성할 수 있게 만드는 기법.
    
    ```java
    public static Boolean valueOf(boolean b) {
           return b ? Boolean.TURE : Boolean.FALSE;
    }
    ```
    

### 정적 팩터리 메서드의 장점

1. 이름을 가질 수 있음.
    - 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하지 못하지만, 정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있음.
        - ex) BigInteger(int, int, Random) vs **BigInteger.probablePrime**
2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 됨.
    - 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있음.
    - ex) Boolean.valueOf(boolean) 메소드는 객체를 아예 생성하지 않음.
3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있음.
4. 입력 매게변수에 따라 매번 다른 클래스의 객체를 반환할 수 있음.
    - 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없음.
    - 클라이언트는 팩터리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도 없고 알 필요도 없음.
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 됨.

### ****정적 팩터리 메서드의 단점****

1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 메서드를 만들 수 없음.
    - 상속이 불가능함.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어려움.

### 정적 팩터리 메서드에 흔히 사용하는 명명 방식

- from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
    - ex) Date d = Date.from(instant);
- of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
    - ex) Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
- valueOf : from과 of의 더 자세한 버전
    - ex) BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
- instance 혹은 getInstance : (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는음.
    - ex) StackWalker luke = StackWalker,getInstance(options);
- create 혹은 newInstance : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장함.
    - ex) Object newArray = Array.newInstance(classObject, arrayLen);
- getType : getInstacne와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 씀. "Type"은 팩터리 메서드가 반환할 객체의 타입임.
    - ex) FileStore fs = Files.getFileStore(path)
- newType : newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 씀. "Type"은 팩터리 메서드가 반환할 객체의 타입.
    - ex) BufferReader br = Files.newBufferedReader(path);
- type : getType과 newType의 간결한 버전
    - ex) List<Complaint> litany = Collections.list(legacyLitany);

<aside>
💡 정적 팩토리 메소드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋음.

</aside>
