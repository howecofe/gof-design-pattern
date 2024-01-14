# Template Method 패턴
```
한줄요약 : "자식 클래스에서 구체적으로 처리한다."
```
## 정의
### 1. 템플릿이란?
```
- 문자 모양대로 구멍이 난 얇은 플라스틱 판
- 그 구멍을 따라 펜/색연필/사인펜 등으로 그리면 종이에 문자가 쓰여진다.
- 핵심: 어떤 필기구를 사용하더라도 쓰여진 문자는 템플릿의 구멍 모양과 같다!
```
### 2. Template Method 패턴이란?
```
- 템플릿 기능을 가진 패턴
- 부모 클래스에 템플릿 기능을 하는 추상 메서드가 정의되어 있다. 추상 메서드이므로 부모 클래스의 코드만 봐서는 어떻게 처리되는지 알 수 없다.
- 자식 클래스는 추상 메서드를 실제로 구현하므로, 구체적인 처리 방식이 정해진다.
- 다른 자식 클래스에서 다르게 구현하면 처리도 다르게 이루어진다.
- 핵심: But 어느 자식 클래스에서 어떻게 구현하더라도 처리의 큰 흐름은 부모 클래스에 정의된 대로!

- 요약: Template Method 패턴이란, 부모 클래스에서 처리의 뼈대를 결정하고 자식 클래스에서 그 구체적 내용을 결정하는 디자인 패턴.
```

## 예제 프로그램
```Template Method 패턴을 사용해서 문자나 문자열을 5번 반복해서 표시하는 프로그램을 만들어보자.```
- ```AbstractDisplay``` : open, print, close, display 중 display 메서드만 구현된 추상 클래스. 📍 display 메서드가 Template Method! 
- ```CharDisplay``` : AbstractDisplay의 자식 클래스, open, print, close를 구현하는 클래스
- ```StringDisplay``` : AbstractDisplay의 자식 클래스, open, print, close를 구현하는 클래스
- ```Main``` : 동작 테스트를 위한 클래스

[AbstractDisplay.java]
```
public abstract class AbstractDisplay {
    // open, print, close는 자식 클래스에 구현을 맡기는 추상 메서드
    public abstract void open();
    public abstract void print();
    public abstract void close();
    
    // display는 AbstractDisplay에서 구현하는 메서드
    // ⭐️⭐️⭐️ 추상 메서드를 사용하는 display가 바로 Template Method !!! ⭐️⭐️⭐️
    public final void display() {
        open();
        for (int i = 0; i < 5; i++) {
            print();
        }
        close();
    }
}
```
[CharDisplay.java]
```
public class CharDisplay extends AbstractDisplay {
    private char ch; // 표시해야 하는 문자

    // 생성자
    public CharDisplay(char ch) {
        this.ch = ch;
    }

    @Override
    public void open() {
        System.out.println("<<");
    }
    @Override
    public void print() {
        System.out.println(ch);
    }
    @Override
    public void close() {
        System.out.println(">>");
    }
}
```
[StringDisplay.java]
```
public class StringDisplay extends AbstractDisplay {
    private String string; // 표시해야 하는 문자열
    private int width; // 문자열의 길이

    // 생성자
    public StringDisplay(String string) {
        this.string = string;
        this.width = string.length();
    }

    @Override
    public void open() {
        printLine();
    }
    @Override
    public void print() {
        System.out.println("|" + string + "|");
    }
    @Override
    public void close() {
        printLine();
    }

    private void printLine() {
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
        System.out.println("+");
    }
}
```
[Main.java]
```
public class Main() {
    public static void main(String[] args) {
        AbstractDisplay d1 = new CharDisplay('H');

        AbstractDisplay d2 = new StringDisplay("Hello, world.");

        // d1, d2 모두 AbstractDisplay의 자식 클래스의 인스턴스이므로 상속받은 display 메서드 호출 가능.
        // 실제 동작은 CharDisplay와 StringDisplay 클래스에서 정해진다.
        d1.display(); ⭐️
        d2.display(); ⭐️
    }
}
```

## Template Method 패턴의 등장인물
### 1. AbstractClass (추상 클래스)
```
- 템플릿 메서드를 구현 (예제에서 display에 해당)
- 그 템플릿 메서드에서 사용할 추상 메서드를 선언 (예제에서 open, print, close에 해당)
- 이 추상 메서드들은 자식 클래스에서 구현된다.
```
### 2. ConcreteClass (구현 클래스)
```
- 추상 클래스를 구현한 자식 클래스
- 자식 클래스에서 구현한 클래스는 추상 클래스의 템플릿 메서드에서 호출된다.
```
#### 클래스 다이어그램
```
[AbstractClass]
    method1
    method2
    method3
    templateMethod

        ⬆️

[ConcreteClass]
    method1
    method2
    method3
```

## 특징
### 1. 장점: 로직을 공통화할 수 있다.
```
반복되는 코드 사용을 줄일 수 있다.
템플릿 메서드의 코드 변경이 필요한 경우, 자식 클래스의 코드를 수정할 필요없이 템플릿 메서드만 수정하면 된다.
```
### 2. 부모 클래스와 자식 클래스의 연계 플레이
```
Template Method 패턴에서는 부모 클래스와 자식 클래스가 긴밀하게 연계하여 움직인다.
따라서 부모 클래스를 잘 이해해야 자식 클래스를 구현할 수 있다.
```
### 3. 다형성: 자식 클래스를 부모 클래스와 동일시한다.
```
부모 클래스의 타입으로 자식 클래스의 인스턴스를 가리킨다. -> 변경에 유리
```

## 관련 패턴
- Factory Method 패턴 : 인스턴스 생성에 Template Method 패턴을 응용
- Strategy 패턴
    - Template Method 패턴에서는 '상속'을 이용하여 프로그램 동작을 변경하는 반면,
    - Strategy 패턴에서는 '위임'을 이용하여 프로그램 일부를 변경하기보다 알고리즘 전체를 모두 전환한다.