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

### 싱글톤 패턴의 단점
- **TDD(Test Driven Development)** 는 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 하며 어떤 순서로든 실행할 수 있어야 함 ➡ TDD를 하기 어려움


