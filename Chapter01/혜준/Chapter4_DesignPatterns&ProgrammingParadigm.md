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
