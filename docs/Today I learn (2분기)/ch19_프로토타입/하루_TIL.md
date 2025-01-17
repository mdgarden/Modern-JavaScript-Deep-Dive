## 📕 오늘 공부한 내용, 이렇게 정리해 봅시다. ✒

**TIL(Today I learn) 기록일** : 2022.08.09

**오늘 읽은 범위** : 19장 프로토타입

## 프로토타입 

### 객채지향 프로그래밍

- 특징이나 성질을 나타내는 속성(attribute/property)을 가지고 있으며 이를 통해 인식하거나 구별할 수 있다.
- 다양한 속성 중 필요한 속성만 간추려 내는 것을 추상화라 한다.
- 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

### 상속과 프로토타입

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172  
```

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172                                                                                     
```

첫 번째 예제와 두 번째 예제의 차이를 요약하자면 아래와 같다

```
인스턴스 생성 시 같은 동작을 하는 메소드를 포함하게 되면 인스턴스 생성 시 마다
동일한 동작을 하는 메서드를 생성하게 된다.

이는 비효율적이고 메모리를 낭비한다.

인스턴스 생성 시 서로 다른 데이터를 기입하고, 같은 동작을 하거나 같은 데이터를 가지는 
프로퍼티 혹은 메서드는 프로토타입에 추가하도록 하자.
```

### 프로토타입

- 프로토타입 체인의 종점은 Object.prototype 이며 ‘__proto__’ 접근자 또한  Object.prototype 의 프로퍼티이다.
- ‘__proto__’ 접근자를 통해 프로토타입에 접근하는 이유는 아무런 체크 없이 프로토타입을 교체할 수 없도록 하기 위함이다.
- prototype 프로퍼티는 함수 객체만이 소유하는데, non-constructor 은 소유하지 않으며 프로토타입도 생성하지 않는다.
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 pair로 존재한다.

### 프로토타입 체인

```jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true
```

- me 함수는 hasOwnProperty 메서드를 가지고 있지 않다. 그러나 프로토타입 체인에 따라 메서드를 검색한다.
- 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이다.
- 스코프 체인은 식별자 검색을 위한 메커니즘이다.
- 스코프 체인과 프로토타입 체인은 별도로 동작하는 것이 아니라 서로 협력한다.
