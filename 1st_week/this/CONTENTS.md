# `this`

> **❗ Keywords** : `binding`, `strict/nonstrict mode`, `Lexical/Dynamic Scope`, `call/apply/bind`

</br>

## `this` 는 무엇인가

---

- 자바스크립트 내에서 `this` 는 '누가 나를 불렀느냐'를 뜻한다고 한다.
- 엄격 모드(ES5)와 비엄격 모드에서 일부 차이가 있다.

</br>

## 상황별 `this` 의 바인딩(binding)

---

상황별로 자바스크립트의 `this` 가 어떻게 **\*바인딩** 되는지 알아보자.

> **바인딩(binding)**: 프로그램의 특정 기본 단위가 가질 수 있는 구성요소의 구체적인 값 내지 성격을 확정하는 것이다. 그러므로 해당 맥락에서는 `this` 의 값을 확정하는 것이라고 할 수 있다.

</br>

### 단독으로 쓴 `this`

---

- **엄격/비엄격 구분 없이 `global object` 를 대상**으로 한다.
- 브라우저에서의 `global object` = `[object Window]`

```javascript
"use strict";

var x = this;
console.log(x); //Window
```

</br>

### 함수 안에서의 `this`

---

- **함수의 주인**에게 `binding` 된다.
  - 비엄격 모드의 경우 함수의 주인은 `[object Window]`
  - 엄격 모드의 경우 `default binding` 이 없으면 `undefined`

</br>

- 비엄격 모드 예시

```javascript
function myFunction() {
  return this;
}
console.log(myFunction()); //Window
```

```javascript
var num = 0;
function addNum() {
  this.num = 100;
  num++;

  console.log(num); // 101
  console.log(window.num); // 101
  console.log(num === window.num); // true
}

addNum();
```

- 엄격 모드 예시

```javascript
"use strict";

function myFunction() {
  return this;
}
console.log(myFunction()); //undefined
```

```javascript
"use strict";

var num = 0;
function addNum() {
  this.num = 100; //ERROR! Cannot set property 'num' of undefined
  // this = undefined 이기 때문에 참조 에러 발생
  num++;
}

addNum();
```

</br>

### 메서드 안에서 쓴 `this`

---

- 해당 **메서드를 호출한 객체**로 `binding` 된다.

```javascript
var person = {
  firstName: "John",
  lastName: "Doe",
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
};

person.fullName(); //"John Doe"
```

```javascript
var num = 0;

function log() {
  console.log(this.num);
}

log(); //0

var obj = {
  num: 200,
  func: log,
};

obj.func(); //200
```

- 호출된 시점에 따라 앞의 `log()` 함수는 `global object` 의 `num` 을 참조하고,
- 아래 `obj.func()` 메서드는 해당 객체(`obj`)의 `num` 을 참조한다.

</br>

### 이벤트 핸들러 안에서의 `this`

---

- 이벤트에 해당하는 `DOM Elements` 로 `binding` 된다.

```javascript
var btn = document.querySelector("#btn");

btn.addEventListener("click", function () {
  console.log(this); //#btn
});
```

- 위 경우 `id` 가 `btn` 인 요소에 `click` 이벤트가 발생할 때마다, 해당 `DOM Element` 를 반환한다.

</br>

### 생성자 안에서 쓴 `this`

---

- 생성자 함수가 생성하는 객체로 `this` 가 `binding` 된다.

```javascript
function Person(name) {
  this.name = name;
}

var kim = new Person("kim");
var lee = new Person("lee");

console.log(kim.name); //kim
console.log(lee.name); //lee
```

- 그러나 생성자 함수가 사라지는 순간, 이는 `global object` 로 `binding` 된다.

```javascript
var name = "window";
function Person(name) {
  this.name = name;
}

var kim = Person("kim"); // 이렇게 되면 여기서 위 this는 전역 객체를 참조한다.

console.log(window.name); //kim
```

</br>

### 명시적 `binding` 을 한 `this`

---

-

```javascript
function whoisThis() {
  console.log(this);
}

whoisThis(); //window

var obj = {
  x: 123,
};

whoisThis.call(obj); //{x:123}
```

</br>

### 참고

> [nana_log : [JS] 자바스크립트에서의 this](https://nykim.work/71)
