## 1.1 자바스크립트의 동등 비교

<aside>
💡 리액트 컴포넌트의 렌더링이 일어나는 이유 중 하나가 바로 props의 동등 비교에 따른 결과다. props의 동등 비교는 객체의 얕은 비교를 기반으로 이뤄진다. 리액트의 가상 DOM과 실제 DOM의 비교, 리액트 컴포넌트가 렌더링할지를 판단하는 방법, 변수나 함수의 메모이제이션 등 모든 작업은 자바스크립트의 동등 비교를 기반으로 한다.

</aside>

### 1.1.1 자바스크립트의 데이터 타입

**원시 타입**

- 자바스크립트에서 원시 타입이란 간단히 정의하자면 객체가 아닌 다른 모든 타입을 의미한다.
- 객체가 아니므로 이러한 타입은 메서들르 갖지 않는다.
- 최신 자바스크립트에서는 총 7개의 원시타입이 있다.

undefined

- undefined는 선언한 후 값을 할당하지 않은 변수 또는 값이 주어지지 않은 인수에 자동으로 할당되는 값이다.

```jsx
let foo;

typeof foo === "undefined"; // true

function bar(hello) {
  return hello;
}

typeof bar() === "undefined"; // true
```

null

- 아직 값이 없거나 비어 있는 값을 표현할 때 사용한다.

```jsx
typeof null === "object"; // true?
```

- null의 특별한 점은 typeof로 null을 확인할 때 해당 타입이 아닌 ‘object’라는 결과가 반환되는 것이다.
- undefined는 ‘선언됐지만 할당되지 않은 값’이고, null은 ‘명시적으로 비어 있음을 나타내는 값’으로 사용하는 것이 일반적이다

Boolean

- 참과 거짓만을 가질 수 있는 데이터 타입이다.
- 주목할 만한 점은 true, false와 같은 boolean 형의 값 외에도 조건문에서 마치 true와 false처럼 취급되는 truthy, falsy값이 존재한다는 것이다.
- falsy : 조건문 내부에서 false로 취급되는 값을 말한다.
- truthy : 조건문 내부에서 true로 취급되는 값, 한 가지 유념할 점은 객체와 배열은 내부에 값이 존재하는지 여부와 상관없이 truthy로 취급된다는 것이다. 즉 {}, [] 모두 truthy한 값이다.

number

BigInt

- number가 다룰 수 있는 숫자 크기의 제한을 극복하기위해 새롭게 나온 타입

String

- string은 텍스트 타입의 데이터를 저장하기 위해 사용된다.
- 자바스크립트 문자열의 특징 중 하나는 문자열이 원시 타입이며 변경 불가능하다는 것이다.

```jsx
const foo = "bar";

console.log(foo[0]); // 'b'

foo[0] = "a";

cosole.log(foo); // bar
```

Symbol

- symbol은 중복되지 않는 어떠한 고유한 값을 나타내기 위해 만들어 졌다. 심벌은 심벌함수를 이용해서만 만들 수 있다.

```jsx
const key = Symbol("key");
const key2 = Symbol("key");

key === key2; // false

// 동일한 값을 사용하기 위해서는 Symbol.for를 활용한다.
Symbol.for("hello") === Symbol.for("hello"); // true
```

객체타입

- 객체타입은 앞서 7가지 원시 타입 이외의 모든 것, 즉 자바스크립트를 이루고 있는 대부분의 타입이 바로 객체 타입이다.
- 한 가지 주목할 것이 객체 타입은 참고를 전달한다고 해서 참조 타입으로도 불린다는 사실이다.

```jsx
typeof [] === "object"; // true
typeof {} === "object"; // true

function hello() {}
typeof hello === "function"; // true

const hello1 = function () {};

const hello2 = function () {};

// 객체인 함수의 내용이 육안으로는 같아 보여도 참조가 다르기 때문에 false가 반환된다.
hello1 === hello2; // false
```

### 1.1.2 값을 저장하는 방식의 차이

- 원시 타입과 객체 타입의 가장 큰 차이점이라고 한다면, 바로 값을 저장하는 방식의 차이다. 이 값을 저장하는 방식의 차이가 동등 비교를 할 때 차이를 만드는 원인이 된다.
- 원시 타입은 불변 형태의 값으로 저장된다. 이 값은 변수 할당 시점에 메모리 영역을 차지하고 저장된다.
- 객체는 프로퍼티를 삭제, 추가 수정할 수 있으므로 원시 값과 다르게 변경 가능한 형태로 저장되며, 값을 복사할 때도 값이 아닌 참조를 전달하게 된다.

```jsx
var hello = {
  greet: "hello, world",
};

var hi = {
  greet: "hello, world",
};

console.log(hello === hi); // false
console.log(hello.greet === hi.greet); // true
```

- 객체는 값을 저장하는 게 아니라 참조를 저장하기 때문에 앞서 동일하게 선언했던 객체라 하더라도 저장하는 순간 다른 참조를 바라보기 때문에 false를 반환하게 된다.
- hello, hi변수는 변수명 및 각 변수명의 조소가 서로 다르지만 value가 가리키는 주소가 동일하다. 즉 value의 값이 변하더라도 비교는 언제나 true이다.

### 1.13 자바스크립트의 또 다른 비교 공식, Object.is

- Object.is는 특별한 사항에서 동등 비교 ===가 가지는 한계를 극복하기 위해 만들어 졌다.

```jsx
-0 === +0 // true
Object.is(-0, +0) // false

Number.NaN === NaN // false
Object.is(Number.NaN, NaN) // true

NaN === 0 / 0 // false
Object.is(NaN, 0 / 0) true
```

- 주의해야 할 점은, Object.is를 사용한다 하더라도 객체 비교에는 별 차이가 없다는 것이다.

### 1.14 리액트에서의 동등 비교

- 리액트에서 사용하는 동등 비교는 Object.is다. 리액트에서는 이를 구현한 폴리필을 함께 사용한다.
- 리액트에서는 objectIs를 기반으로 동등 비교를 하는 shallowEqual이라는 함수를 만들어 사용한다.
- Object.is로 먼저 비교를 수행한 다음 Object.is에서 수행하지 못하는 비교 즉 객체 간 얕은 비교를 한 번 더 수행하는 것을 알 수 있다.

```jsx
// Object.is는 참조가 다른 객체에 대해 비교가 불가능하다.
Object.is({ hello: "world" }, { hello: "world" }); // false

// 반면 리액트 팀에서 구현한 shallowEqual은 객체의 1 depth까지는 비교가 가능하다.
shallowEqual({ hello: "world" }, { hello: "world" }); // true

// 그러나 2 depth까지 가면 이를 비교할 방법이 없으므로 false를 반환한다.
shallowEqual({ hello: { hi: "world" } }, { hello: { hi: "world" } }); // false
```

- 자바스크립트에서 객체 비교의 불완전성은 스칼라나 하스켈 등의 다른 함수형 언어에서는 볼 수 없는 특징으로, 자바스크립트 개발자라면 반드시 기억해 두어야 한다.
- 이러한 자바스크립트의 특징을 잘 숙지한다면 향후 함스형 컴포넌트에서 사용되는 훅의 의존성 배열의 비교, 렌더링 방지를 넘어선 useMemo와 useCallback의 필요성, 렌더링 최적화를 위해서 꼭 필요한 React.memo를 올바르게 작동시키기 위해 고려해야 할 것들을 쉽게 이해할 수 있을 것이다.
