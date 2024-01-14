# Adapter 패턴
```
한줄요약 : "사이에 끼워 재사용한다."
```
## 정의
### 1. Adapter란?
```
- adapt: 적응시키다.
- AC 어댑터는 '직류 12볼트'를 사용하는 컴퓨터를 가정용 전원으로 제공되는 '교류 100볼트' 환경에 '적응시킨다'.
- adapter: 이미 제공된 것(코드)을 그대로 사용할 수 없을 때, 제공된 것과 필요한 것 사이에 들어가서 필요한 형태로 변환한다.
```
### 2. Adapter 패턴이란?
```
- '이미 제공된 것'과 '필요한 것' 사이의 '차이'를 메우는 디자인 패턴
- Wrapper 패턴이라 불리기도 한다. 무언가를 감싸서 다른 용도로 사용할 수 있도록 반환해주는 것이 wrapper이자 adpater의 역할.
```
### 3. 종류
```
- 클래스에 의한 Adapter 패턴 (상속을 사용한 패턴) - 상속관계
- 인스턴스에 의한 Adapter 패턴 (위임을 사용한 패턴) - 포함관계
    * 위임 : 어떤 메서드의 실제 처리를 다른 인스턴스의 메서드에 맡기는 것
```

## 예제 프로그램 (1) 상속을 사용한 패턴 (상속관계)
```"클래스를 사용한" Adapter 패턴을 사용하여, 주어진 문자열을 표시하는 프로그램을 만들어보자.```
- ```Banner 클래스``` : '이미 제공된 것'에 해당. showWithParen(), showWithAster()
- ```Print 인터페이스``` : '필요한 것'에 해당. printWeak(), printStrong()
- 목표: 이미 제공된 Banner 클래스를 사용하여 필요한 Print 인터페이스를 충족하는 클래스를 만드는 것 => 이것이 Adapter!
- ```PrintBanner 클래스``` : '변환 장치' Adapter 역할. Banner 클래스를 상속받아 Print 인터페이스 구현.
    - (showWithParen -> printWeak / showWithAster -> printStrong)
- ```Main 클래스```

[Banner.java]
```
public Class Banner {
    private String string;
    
    public Banner(String string) {
        this.string = string;
    }
    
    public void showWithParen() {
        System.out.println("(" + string + ")");
    }

    public void showWithAster() {
        System.out.println("*" + string + "*");
    }
}
```

[Print.java]
```
public interface Print {
    public abstract void printWeak();
    public abstract void printStrong();
}
```

[PrintBanner.java]
```
public class PrintBanner extends Banner implements Print { // ⭐️⭐️⭐️ Adapter !!! ⭐️⭐️⭐️
    public PrintBanner(String string) {
        super(string);
    }
    
    @Override
    public void printWeak() {
        showWithParen();
    }
    
    @Override
    public void printStrong() {
        showWithAster();
    }
}
```

[Main.java]
```
public class Main {
    public static void main(String[] args) {
        Print p = new PrintBanner("Hello"); // Print 타입이 PrintBanner 인스턴스를 가리킨다!
        p.printWeak();
        p.printStrong();
    }
}
// PrintBanner 클래스가 어떻게 구현됐는지 Main 클래스는 모른다. 따라서 PrintBanner에 변경이 발생해도 Main은 변경하지 않아도 된다. -> 변경에 유리
```

## 예제 프로그램 (2) 위임을 사용한 패턴 (포함관계)
```"인스턴스를 사용한" Adapter 패턴을 사용하여, 주어진 문자열을 표시하는 프로그램을 만들어보자.```
- 목표: Banner 클래스를 이용하여 Print 클래스와 같은 메서드를 갖는 클래스를 실현하려는 것.
- ```Banner와 Main은 앞의 예제와 동일```
- ```Print 클래스``` : 
- ```PrintBanner 클래스``` : 


[Print.java]
```
public abstract class Print {
    public abstract void printWeak();
    public abstract void printStrong();
}
```

[PrintBanner.java]
클래스 다중 상속이 불가능하므로 필드를 통해 Banner 인스턴스를 주입받음.
```
public class PrintBanner extends Print {
    private Banner banner; // ⭐️ has! 포함관계

    public PrintBanner(String string) {
        this.banner = new Banner(string);
    }

    @Override
    public void printWeak() {
        banner.showWithParen(); // 위임 - PrintBanner가 자신이 처리하지 않고, banner의 메서드를 호출해서 맡김.
    }

    @Override
    public void printStrong() {
        banner.showWithAster(); // 위임
    }
}
```

## Adapter 패턴의 등장인물
### 1. Target (대상)
```
- 지금 필요한 메서드 (예제에서는 Print 클래스/인터페이스)
```
### 2. Client (의뢰자)
```
- Target의 메서드를 사용해 일한다. (예제에서는 Main 클래스)
```
### 3. Adaptee (적응 대상자)
```
- 이미 제공된 메서드 (예제에서는 Banner 클래스)
```
### 4. Adapter (적응자)
```
- Adaptee의 메서드를 사용해서 Target을 만족시키는 변환기 역할 (예제에서는 PrintBanner 클래스)
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
### 1. 어떤 경우에 사용하지?
```
- 이미 존재하는 클래스를 이용하고 싶은 경우 빠르게 만들 수 있다.
- 이미 존재하는 클래스(Adaptee)가 충분히 테스트되어 버그가 적다면 더더욱 재사용하고 싶을 것이다.
- 버그가 발생하더라도 Adaptee일 가능성은 적을 것이므로 Adapter를 중점적으로 살펴보면 된다.
```
### 2. 기존 클래스의 소스가 없더라도 새로운 클래스를 만들 수 있다.
```
- Adapter 패턴의 목적은 기존 클래스를 전혀 수정하지 않고 목적한 새로운 인터페이스(API)에 맞추려는 것.
    - (새로운 인터페이스에 맞추려고 하면 기존 클래스를 변경하고 마는데, 그렇게 하면 동작 테스트가 이미 끝난 기존 클래스를 다시 테스트해야 하기 때문이다.)
- 또한 기존 클래스의 소스 프로그램이 반드시 필요한 것은 아니다. 기존 클래스의 사양만 알면 새로운 클래스를 만들 수 있다.
```
### 3. 버전 업과 호환성
```
- 소프트웨어를 버전 업할 때 '구버전과의 호환성'이 문제가 되는데, Adapter 패턴은 신버전과 구버전을 공존시키고 유지보수까지 편하게 한다.
- ex) 신버전만 유지 보수하고 싶은 경우, 신버전을 Adaptee로 구버전을 Target으로 하고,
      신버전의 클래스를 사용하여 구버전의 메서드를 구현하는 Adapter 역할 클래스를 만들면 된다.
    -> 구버전 메서드를 호출하면 신버전 메서드가 호출되므로 신버전만 유지보수할 수 있다.
```
### 4. Adaptee와 Target의 기능이 너무 동떨어진 경우에는 사용할 수 없다.
### 5. 상속과 위임, 어느 쪽을 사용해야 할까?
```
- 일반적으로 위임을 사용하는 것이 문제가 적다.
- why? 부모 클래스의 내부 동작을 자세히 모르면, 상속을 효과적으로 사용하기 어렵기 때문이다.
```

## 관련 패턴
- Bridge 패턴
    - Adapter 패턴 : 인터페이스(API)가 서로 다른 클래스를 연결하는 패턴
    - Bridge 패턴 : 기능 계층과 구현 계층을 연결하는 패턴
- Decorator 패턴
    - Adapter 패턴 : 인터페이스(API)의 차이를 메우는 패턴
    - Decorator 패턴 : 인터페이스를 변경하지 않고 기능을 추가하는 패턴