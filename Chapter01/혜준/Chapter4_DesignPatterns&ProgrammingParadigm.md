# 1.1. 디자인 패턴

### 디자인 패턴

❓ 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것

## 1.1.1. 싱글톤 패턴(singleton pattern)

❓ 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
- 데이터베이스 연결 모듈에 많이 사용
- 장점: 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어듦
- 단점: 의존성이 높아짐

### 자바스크립트의 싱글톤 패턴

```js
const obj = {
  a: 27
}
const obj2 = {
  a: 27
}
console.log(obj === obj2)   //false
```

- obj와 obj2는 서로 다른 메모리에 저장됨 => obj와 obj2는 다른 인스턴스를 가짐

```js
class Singleton {
    constructor() {
        if (!Singleton.instance) {
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance() {
        return this.instance
    }
}
const a = new Singleton()
const b = new Singleton()
console.log(a === b)    //true
```

- 앞의 코드는 Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스를 구현한 모습
- a와 b는 하나의 인스턴스를 가짐

### 데이터베이스 연결 모듈

```js
// DB 연결을 하는 것이기 때문에 비용이 더 높은 작업 
const URL = 'mongodb://localhost:27017/kundolapp' 
const createConnection = url => ({"url" : url})    
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL) 
console.log(a === b) // true
```

- DB.instance라는 하나의 인스턴스를 기반으로 a,b를 생성
- 데이터베이스 연결에 관한 인스턴스 생성 비용을 아낄 수 있음

### MySQL의 싱글톤 패턴

```js
// 메인 모듈
const mysql = require('mysql');
const pool = mysql.createPool({
  connectionLimit: 10,
  host: 'example.org',
  user: 'kundol',
  password: 'secret',
  database: '승철이디비'
 });
 pool.connect();
 
 // 모듈 A
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});

// 모듈 B
pool.query(query, function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
});
```

- 메인 모듈에서 데이터베이스 연결에 관한 인스턴스를 정의
- 다른 모듈인 A 또는 B에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식

### 싱글톤 패턴의 단점

- **TDD(Test Driven Development)** 는 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 어떤 순서로든 실행할 수 있어야 함 ➡ TDD를 하기 어려움
- 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 '독립적인' 인스턴스를 만들기 어려움
- 모듈 간의 결합을 강하게 만들 수 있음

### 의존성 주입

❓ **의존성**: 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는 것을 의미함
- **의존성 주입**(DI, Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결
- 메인 모듈(main mudule)이 '직접' 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자(dependency injector)가 이 부분을 가로채 메인 모듈이 '간접'적으로 의존성을 주입하는 방식 ➡ **디커플링이 됨**: 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어짐

#### 의존성 주입의 장점
- 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션 하기도 수월
- 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어 주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론 가능, 모듈 간의 관계들이 조금 더 명확해짐

#### 의존성 주입의 단점
- 모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가될 수 있음
- 약간의 런타임 패널티가 생기기도 함

#### 의존성 주입 원칙
- "상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
- 둘 다(상위 모듈과 하위 모듈) 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.

## 1.1.2. 팩토리 패턴(factory pattern)
❓ **팩토리 패턴**: 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴

> ex) 
> 하위 클래스: 라떼 레시피와 아메리카노 레시피, 우유 레시피라는 구체적인 내용이 들어있음 <br>
> 상위 클래스: 바리스타 공장 <br>
> 하위 클래스가 컨베이어 벨트를 통해 상위 클래스로 전달하여 이 레시피들을 토대로 우유 등을 생산하는 생산 공정을 생각하면 됨

### 팩토리 패턴 장점
- 상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없음 ➡ 더 많은 유연성을 갖게 됨
- 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 피랙터링하더라도 한 곳만 고칠 수 있음 ➡ 유지 보수성 증가

### 자바스크립트의 팩토리 패턴

- 자바스크립트에서 팩토리 패턴은 `new Object()`로 구현 가능

```js
const num = new Object(42)
const str = new Object('abc')
num.constructor.name; // Number
str.constructor.name; // String
```

```js
class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}   
class Latte {
    constructor() {
        this.name = "latte"
    }
}
class Espresso {
    constructor() {
        this.name = "Espresso"
    }
} 

class LatteFactory extends CoffeeFactory{
    static createCoffee() {
        return new Latte()
    }
}
class EspressoFactory extends CoffeeFactory{
    static createCoffee() {
        return new Espresso()
    }
}
const factoryList = { LatteFactory, EspressoFactory } 
 
 
const main = () => {
    // 라떼 커피를 주문한다.  
    const coffee = CoffeeFactory.createCoffee("LatteFactory")  
    // 커피 이름을 부른다.  
    console.log(coffee.name) // latte
}
main()
```
- CoffeeFactory라는 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스인 LatteFactory가 구체적인 내용을 결정
- 의존성 주입이라고도 볼 수 있음. CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌 LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문
- CoffeeFactory를 보면 static으로 createCoffee() 정적 메서드를 정의한 것을 알 수 있는데, 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점

## 1.1.3. 전략 패턴(strategy pattern)
❓ **전략 패턴**: 정책 패턴(policy pattern)이라고도 하며, 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고 전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴

### passport의 전략 패턴
❓ **passport**: 전략 패턴을 활용한 라이브러리
- Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로, 여러 가지 '전략'을 기반으로 인증할 수 있게 함. 아래와 같은 전략들을 지원함
- **Local Strategy 전략**: 서비스 내의 회원가입된 아이디와 비밀번호를 기반으로 인증
- **OAuth 전략**: 네이버 등 다른 서비스를 기반으로 인증

## 1.1.4. 옵저버 패턴(observer pattern)
❓ **옵저버 패턴**: 주체가 어떤 객체(subject)의 상태 번화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴

> 주체: 객체의 상태 변화를 보고 있는 관찰자 <br>
> 옵저버: 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체

- 주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 함
- 옵저버 패턴을 활용한 서비스로는 트위터가 있다
- 옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller) 패턴에도 사용됨
- 주체라고 볼 수 있는 모델에서 변경 사항이 생겨 `update()` 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러(controller) 등이 작동

#### 자바: 상속과 구현

❓ **상속(extends)**: 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것을 말함. 이로 인해 재사용성, 중복성의 최소화가 이루어짐
❓ **구현(implements)**: 부모 인터페이스를 자식 클래스에서 재정의하여 구현하는 것을 말하며, 상속과는 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야 함
- 상속과 구현의 차이
  - 상속: 일반 클래스, abstract 클래스를 기반으로 구현
  - 구현: 인터페이스를 기반으로 구현
  
### 자바에서의 옵저버 패턴
- Concrete Subject class에서 값을 변경하면 모든 Observer들에게 갱신하는 구조
```java
// Subject interface: Observer를 등록, 해제, 갱신하기 위한 API
public interface Subject
{
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}

// Observer interface: update()함수에 인자로 value값이 넘어옴
public interface Observer
{
    void update(int value);
}

// ConcreteSubject는 Subject interface를 상속 받음. ArrayList를 사용하여 observer 정보를 갖고 있음
public class ConcreteSubject : Subject
{
	IList _observers = new ArrayList();
	int _value;
	public ConcreteSubject()
	{
		_value = 0;
	}
	public void notifyObservers()
	{
		foreach (Observer o in _observers)
			o.update(_value);
	}
 
	public void registerObserver(Observer o)
	{
		_observers.Add(o);
	}
 
	public void removeObserver(Observer o)
	{
		_observers.Remove(o);
	}
	public void setValue(int value)
	{
		_value = value;
		notifyObservers();
	}
}

// 각 Observer interface를 상속받는 A, B, C class. 똑같은 클래스를 중복 시킴. 
// 각 클래스 생성자에서 ConcreteSubject를 인자로 받고, registerObserver를 호출하여 자신을 옵저버로써 등록
public class A : Observer
{
	ConcreteSubject _subject;
	public A(ConcreteSubject subject)
	{
		_subject = subject;
		subject.registerObserver(this);
	}
	public void update(int value)
	{
		Console.WriteLine(String.Format("A Class update, value: {0}", value));
	}
}
 
public class B : Observer
{
	ConcreteSubject _subject;
	public B(ConcreteSubject subject)
	{
		_subject = subject;
		subject.registerObserver(this);
	}
	public void update(int value)
	{
		Console.WriteLine(String.Format("B Class update, value: {0}", value));
	}
}
 
public class C : Observer
{
	ConcreteSubject _subject;
	public C(ConcreteSubject subject)
	{
		_subject = subject;
		subject.registerObserver(this);
	}
	public void update(int value)
	{
		Console.WriteLine(String.Format("C Class update, value: {0}", value));
	}
}

// 메인 프로그램. ConcreteSubject 및 A,B,C 인스턴스를 생성하는데 A, B, C 각 인스턴스가 옵저버로써 ConcreteSubject 인스턴스에 등록됨.
// 마지막 줄인 setValue(10)을 호출하면 모든 옵저버의 update가 호출됨
class Program
{
	static void Main(string[] args)
	{
		ConcreteSubject concreteSubject = new ConcreteSubject();
 
		A observerA = new A(concreteSubject);
		B observerB = new B(concreteSubject);
		C observerC = new C(concreteSubject);
 
		concreteSubject.setValue(10);
	}
}
```

## 1.1.4. 프록시 패턴과 프록시 서버
❓ **프록시 패턴(proxy pattern)**: 대상 객체(subject)에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴
- 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용
- 프록시 객체로 쓰이기도 하지만 프록시 서버로도 활용

### 프록시 서버
❓ **프록시 서버(proxy server)**: 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램을 가리킴

### 이터레이터 패턴(iterator pattern)
❓ **이터레이터 패턴**: 이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴
- 이를 통해 순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회 가능

## 1.1.7. 노출모듈 패턴(revealing module pattern)
❓ **노출모듈 패턴**: 즉시 실행 함수를 통해 private, public과 같은 접근 제어자를 만드는 패턴
- 자바스크립트는 private나 public 같은 접근 제어자가 존재하지 않고 전역 범위에서 스크립트가 실행된다. 그렇기 때문에 노출모듈 패턴을 통해 private와 public 접근 제어자를 구현하기도 함

```js
const pukuba = (() => {
    const a = 1
    const b = () => 2
    const public = {
        c : 2, 
        d : () => 3
    }
    return public 
})() 
console.log(pukuba)
console.log(pukuba.a)
// { c: 2, d: [Function: d] }
// undefined
```

- a와 b는 다른 모듈에서 사용할 수 없는 변수나 함수이며 private 범위를 가짐
- c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수이며 public 범위를 가짐
- 노출모듈 패턴을 기반으로 만든 자바스크립트 모듈 방식: **CJS(commonJS) 모듈 방식**

## 1.1.8. MVC 패턴
❓ **MVC 패턴**: 모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴
- 애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구셩 요소에만 집중해서 개발 가능
- 장점: 재사용성과 확장성이 용이함
- 단점: 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해짐

### 모델(model)
❓ **모델**: 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻함
- 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신

### 뷰(view)
❓ **뷰**: inputbox, checkbox, textarea 등 사용자 인터페이스 요소를 나타냄. = 모델을 기반으로 사용자가 볼 수 있는 화면
- 모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 단순히 사각형 모양 등 화면에 표시하는 정보만 가지고 있어야 함
- 변경이 일어나면  컨트롤러에 이를 전달해야 함

### 컨트롤러(controller)
❓ **컨트롤러**: 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당
- 모델과 뷰의 생명주기도 관리
- 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용에 대해 알려줌

## 1.1.9. MVP 패턴
❓ **MVP 패턴**: MVC 패턴으로부터 파생되었으며 MVC에서 C에 해당하는 컨트롤러가 프레젠터(presenter)로 교체된 패턴
- 뷰와 프레젠터는 일대일 관계이기 대문에 MVC 패턴보다 더 강한 결합을 가진 디자인 패턴이라고 볼 수 있음

## 1.1.10. MVVM 패턴
❓ **MVVM 패턴**: MVC의 C에 해당하는 컨트롤러가 뷰모델(view model)로 바뀐 패턴
- 뷰모델은 뷰를 더 추상화한 계층
- MVVM 패턴은 MVC 패턴과는 다르게 커맨드와 데이터 바인딩을 가지는 것이 특징
- 뷰와 뷰모델 사이의 양방향 데이터 바인딩을 지원
- 장점: UI를 별도의 코드 수정 없이 재사용할수 있고 단위 테스팅하기 쉬움

# 1.2. 프로그래밍 패러다임(programming paradigm)
❓ **프로그래밍 패러다임**: 프로그래머에게 프로그래밍의 관점을 갖게 해주는 역할을 하는 개발 방법론
- 선언형, 명령형으로 나뉨
- 선언형: 함수형이라는 하위 집합을 갖음
- 명령형: 객체지향, 절차지향으로 나뉨

## 1.2.1. 선언형과  함수형 프로그래밍
❓ **선언형 프로그래밍(declarative programming)**: '무엇을' 풀어내는가에 집중하는 패러다임이며, "프로그램은 함수로 이루어진 것이다."라는 명제가 담겨 있는 패러다임
❓ **함수형 프로그래밍(functional programming)**: 선언형 패러다임의 일종으로, 작은 '순수 함수'들을 블록처럼 쌓아 로직을 구현하고 '고차 함수'를 통해 재사용성을 높인 프로그래밍 패러다임
- 자바스크립트는 단순하고 유연한 언어미ㅕ, 함수가 읽브 객체이기 때문에 객체지향 프로그래밍보다는 함수형 프로그래밍 방식이 선호

### 순수 함수
❓ 출력이 입력에만 의존하는 것

### 고차 함수
❓ 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할수 있는 것
- 고차 함수는 일급 객체라는 특징을 가져야 함

#### 일급 객체
1. 변수나 메서드에 함수를 할당할 수 있다.
2. 함수 안에 함수를 매개변수로 담을 수 있다.
3. 함수가 함수를 반환할 수 있다.

## 1.2.2. 객체지향 프로그래밍(OOP, Object-Oriented Programming)
❓ **객체지향 프로그래밍**: 객체들의 집합으로 프로그래밍의 상호 작용을 표현하며 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식
- 설계에 많은 시간이 소요되며 처리 속도가 다른 프로그래밍 패러다임에 비해 상대적으로 느림

### 객체지향 프로그래밍의 특징
- 추상화, 캡슐화, 상속성, 다형성이 있음

#### 추상화(abstraction)
❓ 복잡한 시스템으로부터 핵심적인 개념 또는 기능을 간추려내는 것을 의미

#### 캡슐화(encapsulation)
❓ 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것

#### 상속성(inheritance)
❓ 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
- 코드의 재사용 측면, 계층적인 관계 생성, 유지 보수성 측면에서 중요

#### 다형성(polymorphism)
❓ 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것. 대표적으로 오버로딩, 오버라이딩이 있음
- **오버로딩(overloading)**: 같은 이름을 가진 메서드를 여러 개 두는 것. 메서드의 타입, 매개변수의 유형, 개수 등으로 여러 개를 둘 수 있으며 컴파일 중에 발생하는 '정적' 다형성
-  **오버라이딩(overriding)**: 주로 메서드 오버라이딩을 말하며 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것을 의미. 런타임 중에 발생하는 '동적' 다형성

### 설계원칙(SOLID 원칙)
- **단일 책임 원칙(SRP, Single Responsibility Principle)**: 모든 클래스는 각각 하나의 책임만 가져야 하는 원칙
- **개방-폐쇄 원칙(OCP, Open Closed Principle)**: 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙
- **리스코프 치환 원칙(LSP, Liskov Substitution Principle)**: 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 하는 것을 의미
	- 클래스는 상속이 되기 마련이고 부모, 자식이라는 계층 관계가 만들어지는데, 부모 객체에 자식 객체를 넣어도 시스템이 문제없이 돌아가게 만드는 것
- **인터페이스 분리 원칙(ISP, Interface Segregation Principle)**: 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스를 만들어야 하는 원칙
- **의존 역전 원칙(DIP< Dependency Inversion Principle)**: 자신보다 변하기 쉬운 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 쉬운 것의 변화에 영향받지 않게 하는 원칙
	- 상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야 함을 의미

## 1.2.3. 절차형 프로그래밍
- 절차형 프로그래밍은 로직이 수행되어야 할 연속적인 계산 과정으로 이루어져 있음
- 일이 진행되는 방식으로 그저 코드를 구현하기만 하면 되기 때문에 코드의 가독성이 좋으며 실행 속도가 빠름 ➡ 계산이 많은 작업 등에 쓰임

## 1.2.4. 패러다임의 혼합
- 가장 좋은 패러다임은 없으므로 비즈니스 로직이나 서비스의 특징을 고려해서 패러다임을 정하는 것이 좋음
- 여러 패러다임을 조합하여 상황과 맥락에 따라 패러당미 간의 장점만 취해 개발하는 것이 좋음







