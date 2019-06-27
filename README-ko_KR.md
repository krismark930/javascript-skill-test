# (고급) JavaScript 질문 목록

JavaScript 에 관한 객관식 문제를 [Instagram](https://www.instagram.com/theavocoder)에 매일 게시하고 있어요, 물론 여기에도 게시할 거예요!

초급부터 고급까지: JavaScript를 얼마나 잘 알고 있는지 테스트하거나, 지식을 조금 깊게 하거나, 코딩 면접을 준비하세요! :muscle: :rocket: 이 기록을 매주 새로운 질문으로 업데이트해요. 마지막 업데이트: <b>6월 21일</b>

정답은 질문 아래 접힌 부분에 있어요, 그냥 클릭하면 펼칠 수 있어요. 행운을 빌어요 :heart:

- [English](./README.md)
- [中文版本](./README-zh_CN.md)
- [Versión en español](./README-ES.md)
- [日本語](./README-ja_JA.md)
- [Русский](./README_ru-RU.md)
- [Western Balkan](./README-bs_BS.md)
- [Deutsch](./README-de_DE.md)
- [Tiếng Việt](./README-vi.md)
- [Українська мова](./README-ua_UA.md)
- [Português Brasil](./README_pt_BR.md)

---

###### 1. 무엇이 출력 될까요?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

- A: `Lydia` 그리고 `undefined`
- B: `Lydia` 그리고 `ReferenceError`
- C: `ReferenceError` 그리고 `21`
- D: `undefined` 그리고 `ReferenceError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

함수내에서, 우선 `var`키워드를 사용해 `name`변수를 선언해요. 이것은 변수가 정의되어 있는 행에 실제로 도달할 때까지, 변수는 `undefined`을 기본값으로 호이스팅 되(생성단계에 메모리 공간이 설정됨)는 것을 의미해요. `name` 변수를 출력 하려는 줄에서 아직 변수를 정의하고 있지 않기 때문에, `undefined`값을 유지하고 있어요.

`let`키워드(그리고 `const`)를 가지는 변수들은, `var`와는 달리, 호이스팅되지만 <i>초기화</i>되지 않아요. 그것들을 선언(초기화)하는 줄 전에는 접근 할 수 없어요. 이것은 "일시적 사각지대"라고 불려요. 선언되기 전에 변수에 접근하려고 하면, JavaScript는 `ReferenceError`를 던져요.

</p>
</details>

---

###### 2. 무엇이 출력 될까요?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` 그리고 `0 1 2`
- B: `0 1 2` 그리고 `3 3 3`
- C: `3 3 3` 그리고 `0 1 2`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

JavaScript의 이벤트 큐 때문에, `setTimeout`의 콜백함수는 루프가 실행된 _후에_ 호출되요. 첫번째의 루프 변수 `i`는 `var`키워드를 사용해 선언되어 있으므로, 이 값은 전역변수가 되요. 루프 동안, 단항 연산자 `++`를 사용하여, 매번 `i`의 값을 `1`씩 증가했어요. `setTimeout`콜백 함수가 호출되기까지, 첫번째 예에서 `i`는 `3`과 같아요.

두번째 루프에서, 변수 `i`는 `let`키워드를 사용해 선언되었어요: `let` (그리고 `const`)키워드로 선언된 변수는 블록 범위에요(블록은 `{ }` 사이의 모든것). 각각의 반복동안, `i`는 새로운 값을 가지고, 각각의 값은 루프 안쪽 범위에 있어요.

</p>
</details>

---

###### 3. 무엇이 출력 될까요?

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

shape.diameter();
shape.perimeter();
```

- A: `20` 그리고 `62.83185307179586`
- B: `20` 그리고 `NaN`
- C: `20` 그리고 `63`
- D: `NaN` 그리고 `63`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`diameter`의 값은 정규 함수인 반면, `perimeter`의 값은 화살표 함수라는 점에 유의해요.

화살표 함수에서는, 통상적인 함수와는 달리, `this`키워드는 현재 주위의 범위를 참조해요! 이것은 `perimeter`를 부르면, shape 객체가 아닌, 그 주위의 범위(예를 들면 window)를 참조하는 것을 의미해요.

그 객체에는 `radius`라는 값은 없고, `undefined`를 리턴해요.

</p>
</details>

---

###### 4. 무엇이 출력 될까요?

```javascript
+true;
!"Lydia";
```

- A: `1` 그리고 `false`
- B: `false` 그리고 `NaN`
- C: `false` 그리고 `false`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

단항 더하기는 피연산자를 숫자로 변환하려 해요. `true`는 `1`, `false`은 `0`.

문자열 `'Lydia'`는 참 같은 값이에요. 실제로 묻고 있는 것은, "이 참 같은 값이 거짓 같은 값인가?"에요. 이것은 `false`을 리턴해요.

</p>
</details>

---

###### 5. 정답은 어느 것일까요?

```javascript
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

- A: `mouse.bird.size` 은 유효하지 않아요
- B: `mouse[bird.size]` 은 유효하지 않아요
- C: `mouse[bird["size"]]` 은 유효하지 않아요
- D: 이 모든 것들은 유효해요.

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

JavaScript에서, 모든 객체 키는 문자열이에요 (Symbol이 아닌 한). 비록 그것을 문자열 _형_ 으로 입력하지 않아도, 항상 내부적으로 문자열로 변환되요.

JavaScript는 문장을 해석(또는 박스해제)해요. 대괄호 표기를 사용하면, 첫 번째 좌대괄호 `[`를 보고 오른쪽 대괄호 `]`를 찾을 때까지 진행해요. 그때, 그 문장을 평가할거에요.

`mouse[bird.size]`: 먼저 `bird.size`를 평가해요, 이것은 `"small"`이에요. `mouse["small"]` 은 `true`를 리턴해요.

그러나, 닷 표기에서는, 이것은 발생하지 않아요, `mouse`는 `bird`라고 불리는 키를 가지고 있지 않아요 즉, `mouse.bird`는 `undefined`를 의미해요.

또, 닷 표기를 사용해 `size`를 물어봐요. `mouse.bird.size`. `mouse.bird`는 `undefined`로, 실제로는 `undefined.size`를 물어보고 있어요. 이것은 유효하지 않기 때문에, `Cannot read property "size" of undefined`와 비슷한 에러를 던질거에요.

</p>
</details>

---

###### 6. 무엇이 출력 될까요?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey!`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

JavaScript에서, 모든 객체는 서로 동일하게 설정하면 _참조_ 에 따라 상호작용 해요.

우선 변수 `c`는 객체에 대한 값을 유지해요. 그 후, `c`와 동일한 객체 참조를 `d`에 할당해요.

<img src="https://i.imgur.com/ko5k0fs.png" width="200">

한개의 객체를 변경하면, 그것들 모두 변경해요.

</p>
</details>

---

###### 7. 무엇이 출력 될까요?

```javascript
let a = 3;
let b = new Number(3);
let c = 3;

console.log(a == b);
console.log(a === b);
console.log(b === c);
```

- A: `true` `false` `true`
- B: `false` `false` `true`
- C: `true` `false` `false`
- D: `false` `true` `true`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

`new Number()`는, 내장 함수 생성자에요. 숫자처럼 보이지만, 실제로는 숫자가 아니에요: 많은 추가 특성이 있고 그것은 객체에요.

`==`연산자를 사용할 때, 그건 같은 _값_ 을 가지고 있는지 여부만 확인해요. 그것들은 모두`3`의 값을 가지고 있으므로, `true`를 리턴해요.

그러나, `===`연산자를 사용할 때, 값 _과_ 형 둘다 같아야 해요. 이건 아니에요: `new Number()`는 숫자가 아니에요. **객체**에요. 그래서 둘다 `false`를 리턴해요.

</p>
</details>

---

###### 8. 무엇이 출력 될까요?

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
console.log(freddie.colorChange("orange"));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

`colorChange`함수는 정적이에요. 정적 메소드는 그것들이 만들어지는 생성자 상에서만 살아있도록 설계되어 있어, 어떤 자식들도 상속 받을 수 없어요. `freddie`는 자식이기 때문에, 이 함수는 상속되지 않고, `freddie`인스턴스에서는 이용할 수 없어요: `TypeError`가 던져져요.

</p>
</details>

---

###### 9. 무엇이 출력 될까요?

```javascript
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

객체는 출력되요, 전역객체에 빈 객체를 방금 만들었기 때문이에요. `greeting`을 `greettign`으로 잘못 입력했을 경우, JS 인터프리터는 실제로 이것을 `global.greettign = {}` (또는 브라우저의 `window.greetign = {}`) 라고 간주해요.

이것을 피하기 위해서, `"use strict"`를 사용할 수 있어요. 이렇게 하면 변수를 어떤 것과 동일하게 설정하기 전에 변수를 선언했는지 확인할 수 있어요.

</p>
</details>

---

###### 10. 이것을 하면 어떻게 될까요?

```javascript
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- A: 아무 일도 일어나지 않아요, 이건 완전히 괜찮아요!
- B: `SyntaxError` 이 방법으로 함수에 속성을 추가할 수 없어요.
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

JavaScript에서는 가능해요, 함수는 객체이기 때문이에요!
(윈시형 이외는 모두 객체)

함수는 특별한 종류의 객체에요. 당신이 쓴 코드는 실제 함수가 아니에요. 함수는 속성을 가진 객체에요. 이 속성은 호출이 가능해요.

</p>
</details>

---

###### 11. 무엇이 출력 될까요?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

보통의 객체처럼 생성자에는 속성을 추가할 수 없어요. 한 번에 모든 객체에 기능을 추가하고 싶다면, 프로토타입을 사용해야 해요. 그래서 이 경우에,

```js
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

`member.getFullName()`은 작동되요. 이것은 왜 유익할까요? 이 메소드를 생성자 자체에 추가했다고 할게요. 아마도 모든 `Person` 인스턴스는 이 메소드를 필요로 하지 않을 수도 있어요. 그 경우 그들은 아직 속성을 갖고, 각각의 인스턴스를 위해 메모리 공간을 소비하기 때문에, 많은 메모리 공간을 낭비하게 되요. 대신에, 프로토타입을 추가하는 것만으로, 메모리의 한 지점을 가지지만, 모든 것들은 그것에 접근 할 수 있어요.

</p>
</details>

---

###### 12. 무엇이 출력 될까요?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const lydia = new Person("Lydia", "Hallie");
const sarah = Person("Sarah", "Smith");

console.log(lydia);
console.log(sarah);
```

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` 그리고 `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` 그리고 `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` 그리고 `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` 그리고 `ReferenceError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`sarah`를 위해, `new`키워드를 사용하지 않았어요. `new`를 사용한 경우, 이것은 우리가 만든 새로운 빈 객체를 참조해요. 그러나, `new`를 추가하지 않으면 **전역변수**를 참조하게 되요!

`this.firstName`에 `"Sarah"`을 대입하고, `this.lastName`에 `"Smith"`을 대입 했다고 말했었어요. (그렇지만) 우리는 실제로, `global.firstName = 'Sarah'` 그리고 `global.lastName = 'Smith'`를 정의한거에요. `sarah` 자체는 `undefined`로 남아있어요.

</p>
</details>

---

###### 13. 이벤트 전달의 3단계는 무엇일까요?

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

**capturing** 단계 동안에, 이벤트는 조상요소를 거쳐 목표 요소까지 내려가요. 그런 다음 **target** 요소에 도달하고, **bubbling**이 시작되요.

<img src="https://i.imgur.com/N18oRgd.png" width="200">

</p>
</details>

---

###### 14. 모든 객체는 프로토 타입을 가져요.

- A: true
- B: false

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

**기본객체**를 제외한, 모든 객체는 프로포타입을 가져요. 기본객체는 사용자에 의해 만들어지거나 `new`키워드를 사용하여 만들어져요. 기본객체는 `.toString`와 같은 몇개의 메소드와 속성에 접근할 수 있어요. 이것이 내장 JavaScript 메소드를 사용할 수 있는 이유죠! 이러한 모든 메소드는 프로토 타입에서 이용 할 수 있어요. JavaScript가 당신의 객체를 직접 찾을 수 없더라도, 당신이 접근 할 수 있도록, 프로토타입 체인으로 내려가서 찾을거에요.

</p>
</details>

---

###### 15. 무엇이 출력 될까요?

```javascript
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

- A: `NaN`
- B: `TypeError`
- C: `"12"`
- D: `3`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

JavaScript는 **동적으로 만들어진 언어**에요: 특정변수가 어떤 형인지 지정하지 않아요. 변수는 당신이 모르는 사이에 자동으로 다른 형으로 변환 될 수 있는데, 이걸 _암묵적 형 변환_ 이라고 불러요. **Coercion**은 하나의 형을 다른 형으로 변환하는 거에요.

이 예제에서, 함수가 이해하고 값을 리턴하도록, JavaScript는 숫자 `1`을 문자열로 변환해요. 수형 (`1`)와 문자열형 (`'2'`)의 추가 중에는, 숫자는 문자열로 취급되요. `"Hello" + "World"`처럼 문자열을 연결할 수 있어요, 따라서 여기 `"1" + "2"`는 `"12"`을 리턴하는 일이 발생해요.

</p>
</details>

---

###### 16. 무엇이 출력 될까요?

```javascript
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

- A: `1` `1` `2`
- B: `1` `2` `2`
- C: `0` `2` `2`
- D: `0` `1` `2`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

**접미사** 단항연산자 `++`:

1. 값 리턴 (이것은 `0`을 리턴해요)
2. 값 증가 (number는 지금 `1`이에요)

**접두사** 단항연산자 `++`:

1. 값 증가 (number는 지금 `2`이에요)
2. 값 리턴 (이것은 `2`을 리턴해요)

이건 `0 2 2`을 리턴해요.

</p>
</details>

---

###### 17. 무엇이 출력 될까요?

```javascript
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = "Lydia";
const age = 21;

getPersonInfo`${person} is ${age} years old`;
```

- A: `"Lydia"` `21` `["", " is ", " years old"]`
- B: `["", " is ", " years old"]` `"Lydia"` `21`
- C: `"Lydia"` `["", " is ", " years old"]` `21`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

태그드 템플릿 리터럴을 사용하는 경우, 첫번재 인수의 값은 항상 문자열 값의 배열이에요. 나머지 인수는 식을 통과한 값을 얻게 되요.

</p>
</details>

---

###### 18. 무엇이 출력 될까요?

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("You are an adult!");
  } else if (data == { age: 18 }) {
    console.log("You are still an adult.");
  } else {
    console.log(`Hmm.. You don't have an age I guess`);
  }
}

checkAge({ age: 18 });
```

- A: `You are an adult!`
- B: `You are still an adult.`
- C: `Hmm.. You don't have an age I guess`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

동등성을 테스트할 때, 원시형은 그 _값_ 에 따라 비교되며, 객체는 그들의 _참조_ 에 따라 비교되요. JavaScript 객체가 메모리내의 같은 장소를 참조하고 있는지 여부를 확인해요.

비교하고 있는 두개의 객체는 그것이 없어요: 파라미터로 전달된 객체와 동등성을 확인하기 위해 사용한 객체는 메모리 내의 다른 장소를 참조해요.

이것이 `{ age: 18 } === { age: 18 }` 그리고 `{ age: 18 } == { age: 18 }` 두개 다 `false`를 리턴하는 이유죠.

</p>
</details>

---

###### 19. 무엇이 출력 될까요?

```javascript
function getAge(...args) {
  console.log(typeof args);
}

getAge(21);
```

- A: `"number"`
- B: `"array"`
- C: `"object"`
- D: `"NaN"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

스프레드 연산자 (`...args`.)는 인수를 가진 배열을 리턴해요. 배열은 객체이므로, `typeof args`는 `"object"`를 리턴해요.

</p>
</details>

---

###### 20. 무엇이 출력 될까요?

```javascript
function getAge() {
  "use strict";
  age = 21;
  console.log(age);
}

getAge();
```

- A: `21`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

`"use strict"`을 사용하면, 실수로 전역 변수를 선언하지 않게 할 수 있어요. `age`이라는 변수를 선언한적이 전혀없고, `"use strict"`을 사용하고 있으므로, 참조 에러를 던지게 될거에요. 만약 `"use strict"`을 사용하지 않았다면, 이건 작동할거에요, `age` 속성이 전역 객체에 추가된 것이기 때문이죠.

</p>
</details>

---

###### 21. `sum`의 값은 무엇일까요?

```javascript
const sum = eval("10*10+5");
```

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`eval` 문자열로서 통과된 코드를 평가해요. 이 경우와 같이 만약 그것이 표현식이라면, 표현식을 평가해요. 표현식은 `10 * 10 + 5` 이에요. 이것은 숫자 `105`를 리턴해요.

</p>
</details>

---

###### 22. cool_secret에 몇시간 접근이 가능 할까요 ?

```javascript
sessionStorage.setItem("cool_secret", 123);
```

- A: 영원히, 데이터는 사라지지 않아요.
- B: 사용자가 탭을 닫을 때.
- C: 사용자가 탭 뿐만 아니라, 브라우저 전체를 닫을 때.
- D: 사용자가 자신의 컴퓨터를 종료시켰을때.

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`sessionStorage`에 저장된 데이터는 _탭_ 을 닫은 후에 삭제되요.

만약 `localStorage`를 사용했다면, 예를들어 `localStorage.clear()`를 호출 하지 않는 한, 데이터는 영원할거에요.

</p>
</details>

---

###### 23. 무엇이 출력 될까요?

```javascript
var num = 8;
var num = 10;

console.log(num);
```

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`var`키워드를 사용하면, 같은 이름으로 복수의 변수를 선언 할 수 있어요. 변수는 최신의 값을 유지해요.

블록 스코프의 `let` 또는 `const`에서는 할 수 없어요.

</p>
</details>

---

###### 24. 무엇이 출력 될까요?

```javascript
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

모든 객체 키는(Symbol 제외) 문자열로 직접 입력하지 않아도, 내부적으로는 문자열이에요. 이것이 `obj.hasOwnProperty('1')` 또한 true를 리턴하는 이유에요.

set에서는 작동하지 않아요. set에는 `'1'`이 없어요: `set.has('1')`는 `false`를 리턴해요. 그것은 수형인 `1`을 가지고 있어, `set.has(1)`은 `true`를 리턴해요.

</p>
</details>

---

###### 25. 무엇이 출력 될까요?

```javascript
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

같은 이름의 키를 두개 가지고 있다면, 첫번째 위치에서, 마지막에 지정된 값으로 대체 될 거에요.

</p>
</details>

---

###### 26. JavaScript의 global execution context는 두개를 작성해요. : 전역객체와 "this" 키워드에요.

- A: true
- B: false
- C: it depends

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

기본적인 execution context는 전역 실행 문장이에요: 당신의 코드 모든 곳에서 접근 할 수 있어요.

</p>
</details>

---

###### 27. 무엇이 출력 될까요?

```javascript
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

- A: `1` `2`
- B: `1` `2` `3`
- C: `1` `2` `4`
- D: `1` `3` `4`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

`continue` 표현식은 특정 조건이 `true`를 리턴하면 반복처리를 건너뛰어요.

</p>
</details>

---

###### 28. 무엇이 출력 될까요?

```javascript
String.prototype.giveLydiaPizza = () => {
  return "Just give Lydia pizza already!";
};

const name = "Lydia";

name.giveLydiaPizza();
```

- A: `"Just give Lydia pizza already!"`
- B: `TypeError: not a function`
- C: `SyntaxError`
- D: `undefined`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`String`은 내장 생성자로 속성을 추가 할 수 있어요. 단지 프로토타입이라는 메소드를 추가했어요. 원시형 문자열은 문자열 프로토타입 함수에 의해 생성된 문자열 객체로 자동 변환되요. 그래서, 모든 문자열(문자열 객체)는 그 메소드에 접근 할 수 있어요!

</p>
</details>

---

###### 29. 무엇이 출력 될까요?

```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

- A: `123`
- B: `456`
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

객체 키는 자동으로 문자열로 변환되요. 객체 `a`의 키 값으로 `123`를 세팅하려고 해요.

그러나, 객체를 문자열화 하면 `"[Object object]"`가 되요. 그래서 여기서 밀하고자 하는 건 `a["Object object"] = 123` 이라는 거에요. 그후, 같은 일을 다시 시도해요. `c`는 암묵적으로 문자열화 한 다른객체에요. 그래서 `a["Object object"] = 456`이 되요.

그후, `a[b]`는 출력하면 실제로는 `a["Object object"]`에요. 단지 `456`를 설정 했기때문에, `456`를 리턴해요.

</p>
</details>

---

###### 30. 무엇이 출력 될까요?

```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Third");

bar();
foo();
baz();
```

- A: `First` `Second` `Third`
- B: `First` `Third` `Second`
- C: `Second` `First` `Third`
- D: `Second` `Third` `First`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`setTimeout`함수를 처음으로 호출 했어요. 그러나 그것은 마지막에 출력되요.

브라우저에는 런타임 엔진 뿐만 아니라 `WebAPI`라고 불리는 것도 있기 때문이에요. `WebAPI`는 `setTimeout`함수를 최초에 부여하는데, DOM을 예로 들 수 있어요.

After the _callback_ is pushed to the WebAPI, the `setTimeout` function itself (but not the callback!) is popped off the stack.

_callback_ 이 WebAPI에 푸시된 후, `setTimeout`함수 자체(callback이 아니에요!)는 stack에 사라졌어요.

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

지금, `foo` 는 호출 되었고, `"First"`는 출력 되었어요.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo`는 stack에 사라지고, `baz`가 호출 되었어요. `"Third"`가 출력 되었어요.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

WebAPI는 준비가 될때 마다 stack에 항목을 추가 할 수 없어요. 대신에, _queue_ 라고 불리는 것에 callback함수를 푸시해요.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

여기서 event loop가 작동하기 시작해요. **event loop**는 stack과 task queue를 봐요. stack이 비어있다면, queue에 첫번째것을 가져다가 stack 위로 푸시해요.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

`bar`가 호출되었고, `"Second"`가 출력 되었으며, stack에서 사라졌어요.

</p>
</details>

---

###### 31. 버튼을 클릭했을때 event.target은 무엇일까요?

```html
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: 외부의 `div`
- B: 내부의 `div`
- C: `button`
- D: 중첩된 모든 요소의 배열

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

가장 깊이 중첩된 요소가 이벤트를 발생시킬 이벤트 대상이에요. `event.stopPropagation`을 통해서 버블링을 중단 할 수 있어요.

</p>
</details>

---

###### 32. p태그를 클릭하면 로그의 출력은 무엇일까요 ?

```html
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```

- A: `p` `div`
- B: `div` `p`
- C: `p`
- D: `div`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`p`를 클릭하면, 2개의 로그를 볼 수 있어요: `p` 그리고 `div`. 이벤트의 전파 중에는 3 단계가 있어요: 캡처링, 타켓, 버블링. 기본적으로, 이벤트 핸들러는 버블링단계에서 시작되요. (`useCapture`를 `true`로 설정하지 않는 한). 가장 깊게 중첩된 요소로 부터 바깥쪽으로 나가요.

</p>
</details>

---

###### 33. 무엇이 출력 될까요?

```javascript
const person = { name: "Lydia" };

function sayHi(age) {
  console.log(`${this.name} is ${age}`);
}

sayHi.call(person, 21);
sayHi.bind(person, 21);
```

- A: `undefined is 21` `Lydia is 21`
- B: `function` `function`
- C: `Lydia is 21` `Lydia is 21`
- D: `Lydia is 21` `function`

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

두개 모두, `this`키워드를 참조하고자하는 객체로 보낼 수 있어요. 그러나, `.call`은 _즉시 실행되요_!

`.bind.`는 함수의 _복사본_ 을 리턴하지만, 바인딩 컨텍스트에요! 이건 즉시 실행되지 않아요.

</p>
</details>

---

###### 34. 무엇이 출력 될까요?

```javascript
function sayHi() {
  return (() => 0)();
}

console.log(typeof sayHi());
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`sayHi`함수는 즉시 호출 함수(IIFE)로서 리턴된 값을 리턴해요. 이 함수는 `0`을 리턴하고, 형은 `"number"`이에요.

참고: 단 7개의 내장형이 있어요: `null`, `undefined`, `boolean`, `number`, `string`, `object` 그리고 `symbol`. `"function"`은 객체이기 때문에 형이아니라 `"object"` 형이에요.

</p>
</details>

---

###### 35. 이 값들 중 어느 것이 거짓 같은 값 일까요?

```javascript
0;
new Number(0);
("");
(" ");
new Boolean(false);
undefined;
```

- A: `0`, `''`, `undefined`
- B: `0`, `new Number(0)`, `''`, `new Boolean(false)`, `undefined`
- C: `0`, `''`, `new Boolean(false)`, `undefined`
- D: 모든 값은 거짓

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

단 6개의 거짓 값이 있어요:

- `undefined`
- `null`
- `NaN`
- `0`
- `''` (빈 문자열)
- `false`

`new Number` 그리고 `new Boolean`와 같은 생성자 함수는 참같은 값이에요.

</p>
</details>

---

###### 36. 무엇이 출력 될까요?

```javascript
console.log(typeof typeof 1);
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`typeof 1` 은 `"number"`을 리턴해요.
`typeof "number"`은 `"string"`을 리턴해요.

</p>
</details>

---

###### 37. 무엇이 출력 될까요?

```javascript
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

배열의 길이를 초과한 값을 배열의 요소로 설정하고자 할때, JavaScript는 "empty slots"이라고 불리는 것을 생성해요. 이것은 실제로 `undefined`의 값을 가지고 있지만, 다음과 같은 것을 보게 될거에요:

`[1, 2, 3, 7 x empty, 11]`

depending on where you run it (it's different for every browser, node, etc.)
실행 위치에 따라 달라요 (브라우저, node 등 마다 달라요.)

</p>
</details>

---

###### 38. 무엇이 출력 될까요?

```javascript
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

- A: `1` `undefined` `2`
- B: `undefined` `undefined` `undefined`
- C: `1` `1` `2`
- D: `1` `undefined` `undefined`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`catch`블록은 `x`의 인자를 받아요. 이것은 인수를 전달할때 변수로서의 `x`와는 달라요. 이 `x` 변수는 블록-스코프에요.

후에, 블록-스코프 변수는 `1`로 설정하고, 변수 `y`의 값을 설정해요. 여기서, 블록-스코프의 변수 `x`를 출력하는데, 이것은 `1`이에요.

`catch` 블록 밖에서, `x`는 여전히 `undefined`이고 `y`는 `2`이에요. `catch` 블록 밖에서 `console.log(x)`를 출력하면, `undefined`을 리턴하고. 그리고 `y`는 `2`를 리턴해요.

</p>
</details>

---

###### 39. JavaScript의 모든 것은...

- A: primitive 또는 object
- B: function 또는 object
- C: 함정 문제! objects만
- D: number 또는 object

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

JavaScript는 원시형과 객체만 가지고 있어요.

원시형은 `boolean`, `null`, `undefined`, `bigint`, `number`, `string` 그리고 `symbol`이 있어요.

원시형과 객체를 구별하는 법은 원시형에는 속성이나 메소드가 없어요. 그러나 `'foo'.toUpperCase()`는 `'FOO'`로 평가되어, `TypeError`의 결과가 되지 않아요. 문자열과 같은 원시형이 속성 또는 메소드에 접근하려고 할때, JavaScript는 래퍼 클래스 중 하나인 `String`을 사용하여 암묵적으로 감싸고, 표현식이 평가된 후 즉시 래퍼를 폐기하기 때문이에요. `null` 그리고 `undefined`를 제외한 모든 원시형은 이러한 행동을 합니다.

</p>
</details>

---

###### 40. 무엇이 출력 될까요?

```javascript
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

- A: `[0, 1, 2, 3, 1, 2]`
- B: `[6, 1, 2]`
- C: `[1, 2, 0, 1, 2, 3]`
- D: `[1, 2, 6]`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

`[1, 2]`은 초기값이에요. 이것이 최초의 값으로, 제일 처음의 `acc`의 값이에요. 처음 라운드 동안에 `acc`은 `[1,2]`이며, `cur`은 `[0, 1]`이에요. 그것들을 연결하면 결과적으로 `[1, 2, 0, 1]`이 되요.

그리고나서, `[1, 2, 0, 1]`은 `acc`이고, `[2, 3]`은 `cur`이 에요. 그것들을 연결하면 `[1, 2, 0, 1, 2, 3]`을 얻게되요.

</p>
</details>

---

###### 41. 무엇이 출력 될까요?

```javascript
!!null;
!!"";
!!1;
```

- A: `false` `true` `false`
- B: `false` `false` `true`
- C: `false` `true` `true`
- D: `true` `true` `false`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

`null`은 거짓 같은 값이에요. `!null`은 `true`를 리턴해요. `!true`은 `false`를 리턴해요.

`""` 은 거짓 같은 값이에요. `!""`은 `true`를 리턴해요. `!true`은 `false`를 리턴해요.

`1`은 참 같은 값이에요. `!1`은 `false`를 리턴해요. `!false`는`true`를 리턴해요.

</p>
</details>

---

###### 42. `setInterval` 메소드는 브라우저에게 무엇을 리턴 할까요?

```javascript
setInterval(() => console.log("Hi"), 1000);
```

- A: 유니크한 id
- B: 지정된 밀리초
- C: 통과된 함수
- D: `undefined`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

이것은 유니크한 id를 리턴해요. 이 id는 `clearInterval()` 함수로 간격을 없애기 위해 사용 될 수 있어요.

</p>
</details>

---

###### 43. 이것은 무엇을 리턴할까요?

```javascript
[..."Lydia"];
```

- A: `["L", "y", "d", "i", "a"]`
- B: `["Lydia"]`
- C: `[[], "Lydia"]`
- D: `[["L", "y", "d", "i", "a"]]`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

문자열은 반복 가능한 객체에요. 스프레드 연산자는 반복 가능한 객체의 모든 문자를 1개의 요소로 매핑해요.

</p>
</details>

---

###### 44. 무엇이 출력 될까요?

```javascript
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);
console.log(gen.next().value);
```

- A: `[0, 10], [10, 20]`
- B: `20, 20`
- C: `10, 20`
- D: `0, 10 and 10, 20`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

Regular functions cannot be stopped mid-way after invocation. However, a generator function can be "stopped" midway, and later continue from where it stopped. Every time a generator function encounters a `yield` keyword, the function yields the value specified after it. Note that the generator function in that case doesn’t _return_ the value, it _yields_ the value.

First, we initialize the generator function with `i` equal to `10`. We invoke the generator function using the `next()` method. The first time we invoke the generator function, `i` is equal to `10`. It encounters the first `yield` keyword: it yields the value of `i`. The generator is now "paused", and `10` gets logged.

Then, we invoke the function again with the `next()` method. It starts to continue where it stopped previously, still with `i` equal to `10`. Now, it encounters the next `yield` keyword, and yields `i * 2`. `i` is equal to `10`, so it returns `10 * 2`, which is `20`. This results in `10, 20`.

</p>
</details>

---

###### 45. 이것은 무엇을 리턴할까요?

```javascript
const firstPromise = new Promise((res, rej) => {
  setTimeout(res, 500, "one");
});

const secondPromise = new Promise((res, rej) => {
  setTimeout(res, 100, "two");
});

Promise.race([firstPromise, secondPromise]).then(res => console.log(res));
```

- A: `"one"`
- B: `"two"`
- C: `"two" "one"`
- D: `"one" "two"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

When we pass multiple promises to the `Promise.race` method, it resolves/rejects the _first_ promise that resolves/rejects. To the `setTimeout` method, we pass a timer: 500ms for the first promise (`firstPromise`), and 100ms for the second promise (`secondPromise`). This means that the `secondPromise` resolves first with the value of `'two'`. `res` now holds the value of `'two'`, which gets logged.

</p>
</details>

---

###### 46. 무엇이 출력 될까요?

```javascript
let person = { name: "Lydia" };
const members = [person];
person = null;

console.log(members);
```

- A: `null`
- B: `[null]`
- C: `[{}]`
- D: `[{ name: "Lydia" }]`

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

First, we declare a variable `person` with the value of an object that has a `name` property.

<img src="https://i.imgur.com/TML1MbS.png" width="200">

Then, we declare a variable called `members`. We set the first element of that array equal to the value of the `person` variable. Objects interact by _reference_ when setting them equal to each other. When you assign a reference from one variable to another, you make a _copy_ of that reference. (note that they don't have the _same_ reference!)

<img src="https://i.imgur.com/FSG5K3F.png" width="300">

Then, we set the variable `person` equal to `null`.

<img src="https://i.imgur.com/sYjcsMT.png" width="300">

We are only modifying the value of the `person` variable, and not the first element in the array, since that element has a different (copied) reference to the object. The first element in `members` still holds its reference to the original object. When we log the `members` array, the first element still holds the value of the object, which gets logged.

</p>
</details>

---

###### 47. 무엇이 출력 될까요?

```javascript
const person = {
  name: "Lydia",
  age: 21
};

for (const item in person) {
  console.log(item);
}
```

- A: `{ name: "Lydia" }, { age: 21 }`
- B: `"name", "age"`
- C: `"Lydia", 21`
- D: `["name", "Lydia"], ["age", 21]`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

With a `for-in` loop, we can iterate through object keys, in this case `name` and `age`. Under the hood, object keys are strings (if they're not a Symbol). On every loop, we set the value of `item`equal to the current key it’s iterating over. First, `item` is equal to `name`, and gets logged. Then, `item` is equal to `age`, which gets logged.

</p>
</details>

---

###### 48. 무엇이 출력 될까요?

```javascript
console.log(3 + 4 + "5");
```

- A: `"345"`
- B: `"75"`
- C: `12`
- D: `"12"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

Operator associativity is the order in which the compiler evaluates the expressions, either left-to-right or right-to-left. This only happens if all operators have the _same_ precedence. We only have one type of operator: `+`. For addition, the associativity is left-to-right.

`3 + 4` gets evaluated first. This results in the number `7`.

`7 + '5'` results in `"75"` because of coercion. JavaScript converts the number `7` into a string, see question 15. We can concatenate two strings using the `+`operator. `"7" + "5"` results in `"75"`.

</p>
</details>

---

###### 49. What's the value of `num`?

```javascript
const num = parseInt("7*6", 10);
```

- A: `42`
- B: `"42"`
- C: `7`
- D: `NaN`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

Only the first numbers in the string is returned. Based on the _radix_ (the second argument in order to specify what type of number we want to parse it to: base 10, hexadecimal, octal, binary, etc.), the `parseInt` checks whether the characters in the string are valid. Once it encounters a character that isn't a valid number in the radix, it stops parsing and ignores the following characters.

`*` is not a valid number. It only parses `"7"` into the decimal `7`. `num` now holds the value of `7`.

</p>
</details>

---

###### 50. What's the output`?

```javascript
[1, 2, 3].map(num => {
  if (typeof num === "number") return;
  return num * 2;
});
```

- A: `[]`
- B: `[null, null, null]`
- C: `[undefined, undefined, undefined]`
- D: `[ 3 x empty ]`

<details><summary><b>정답</b></summary>
<p>

#### 정답: C

When mapping over the array, the value of `num` is equal to the element it’s currently looping over. In this case, the elements are numbers, so the condition of the if statement `typeof num === "number"` returns `true`. The map function creates a new array and inserts the values returned from the function.

However, we don’t return a value. When we don’t return a value from the function, the function returns `undefined`. For every element in the array, the function block gets called, so for each element we return `undefined`.

</p>
</details>

---

###### 51. 무엇이 출력 될까요?

```javascript
function getInfo(member, year) {
  member.name = "Lydia";
  year = 1998;
}

const person = { name: "Sarah" };
const birthYear = "1997";

getInfo(person, birthYear);

console.log(person, birthYear);
```

- A: `{ name: "Lydia" }, "1997"`
- B: `{ name: "Sarah" }, "1998"`
- C: `{ name: "Lydia" }, "1998"`
- D: `{ name: "Sarah" }, "1997"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

Arguments are passed by _value_, unless their value is an object, then they're passed by _reference_. `birthYear` is passed by value, since it's a string, not an object. When we pass arguments by value, a _copy_ of that value is created (see question 46).

The variable `birthYear` has a reference to the value `"1997"`. The argument `year` also has a reference to the value `"1997"`, but it's not the same value as `birthYear` has a reference to. When we update the value of `year` by setting `year` equal to `"1998"`, we are only updating the value of `year`. `birthYear` is still equal to `"1997"`.

The value of `person` is an object. The argument `member` has a (copied) reference to the _same_ object. When we modify a property of the object `member` has a reference to, the value of `person` will also be modified, since they both have a reference to the same object. `person`'s `name` property is now equal to the value `"Lydia"`

</p>
</details>

---

###### 52. 무엇이 출력 될까요?

```javascript
function greeting() {
  throw "Hello world!";
}

function sayHi() {
  try {
    const data = greeting();
    console.log("It worked!", data);
  } catch (e) {
    console.log("Oh no an error!", e);
  }
}

sayHi();
```

- A: `"It worked! Hello world!"`
- B: `"Oh no an error: undefined`
- C: `SyntaxError: can only throw Error objects`
- D: `"Oh no an error: Hello world!`

<details><summary><b>정답</b></summary>
<p>

#### 정답: D

With the `throw` statement, we can create custom errors. With this statement, you can throw exceptions. An exception can be a <b>string</b>, a <b>number</b>, a <b>boolean</b> or an <b>object</b>. In this case, our exception is the string `'Hello world'`.

With the `catch` statement, we can specify what to do if an exception is thrown in the `try` block. An exception is thrown: the string `'Hello world'`. `e` is now equal to that string, which we log. This results in `'Oh an error: Hello world'`.

</p>
</details>

---

###### 53. 무엇이 출력 될까요?

```javascript
function Car() {
  this.make = "Lamborghini";
  return { make: "Maserati" };
}

const myCar = new Car();
console.log(myCar.make);
```

- A: `"Lamborghini"`
- B: `"Maserati"`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>정답</b></summary>
<p>

#### 정답: B

When you return a property, the value of the property is equal to the _returned_ value, not the value set in the constructor function. We return the string `"Maserati"`, so `myCar.make` is equal to `"Maserati"`.

</p>
</details>

---

###### 54. 무엇이 출력 될까요?

```javascript
(() => {
  let x = (y = 10);
})();

console.log(typeof x);
console.log(typeof y);
```

- A: `"undefined", "number"`
- B: `"number", "number"`
- C: `"object", "number"`
- D: `"number", "undefined"`

<details><summary><b>정답</b></summary>
<p>

#### 정답: A

`let x = y = 10;` is actually shorthand for:

```javascript
y = 10;
let x = y;
```

When we set `y` equal to `10`, we actually add a property `y` to the global object (`window` in browser, `global` in Node). In a browser, `window.y` is now equal to `10`.

Then, we declare a variable `x` with the value of `y`, which is `10`. Variables declared with the `let` keyword are _block scoped_, they are only defined within the block they're declared in; the immediately-invoked function (IIFE) in this case. When we use the `typeof` operator, the operand `x` is not defined: we are trying to access `x` outside of the block it's declared in. This means that `x` is not defined. Values who haven't been assigned a value or declared are of type `"undefined"`. `console.log(typeof x)` returns `"undefined"`.

However, we created a global variable `y` when setting `y` equal to `10`. This value is accessible anywhere in our code. `y` is defined, and holds a value of type `"number"`. `console.log(typeof y)` returns `"number"`.

</p>
</details>
