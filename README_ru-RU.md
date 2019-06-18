# Список (продвинутых) вопросов по JavaScript

Я ежедевно публикую вопросы по JavaScript с вариантами ответов в своем [Instagram](https://www.instagram.com/theavocoder), которые дублируются в этом репозитории.

От азов к сложным вопросам: провеь как хорошо ты знаешь JavaScript, освежи свои знания или приготовься к собеседованию! :muscle: :rocket: Я дополняю этот репозиторий каждую неделю новыми вопросами.

Ответы находятся в свернутой секции ниже вопросов. Просто нажми на ответ, чтобы развернуть. Удачи! :heart:

[中文版本](./README-zh_CN.md)  
[Русский](./README_ru-RU.md)

---

###### 1. Что будет в консоли?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

- A: `Lydia` и `undefined`
- B: `Lydia` и `ReferenceError`
- C: `ReferenceError` и `21`
- D: `undefined` и `ReferenceError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: D

Внутри функции мы сперва определям переменную `name` с помощью ключевого слова `var`. Это означает, что переменная будет поднята (область памяти под переменную будет выделена во время фазы создания) со значением `undefined` по умолчанию, до тех пора пока исполнение кода не дойдет до строчки, где определяется переменная. Мы еще не определили значение `name` когда пытаемся вывести её в консоль, поэтому в консоли будет `undefined`.

Переменные, определенные с помощью `let` (и `const`), также поднимаются, но в отличие от `var`, не <i>инициализируются</i>. Доступ к ним не возможен до тех пор, пока не выполнится строка их определения (инициализации). Это называется "временная мертвая зона". Когда мы пытаемся обратиться к переменным до того момента как они определены, JavaScript выбрасывает исключение `ReferenceError`.

</p>
</details>

---

###### 2. Что будет в консоли?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` и `0 1 2`
- B: `0 1 2` и `3 3 3`
- C: `3 3 3` и `0 1 2`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Из-за очереди событий в JavaScript, функция `setTimeout` вызывается _после_ того как цикл будет завершен. Так как переменная `i` в первом цикле была определена с помощью `var`, она будет глобальной. В цикле мы каждый раз увеличиваем значение `i` на `1`, используя унарный оператор `++`. К моменту выполнения функции `setTimeout` значение `i` будет равно `3` в первом примере.

Во втором цикле переменная `i` определена с помощью `let`. Такие переменные (а также `const`) имеют блочную область видимости (блок это что угодно между `{ }`). С каждой итерацией `i` будет иметь новое значение, и каждое значение будет замкнуто в своей области видимости внутри цикла.

</p>
</details>

---

###### 3. Что будет в консоли?

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

- A: `20` и `62.83185307179586`
- B: `20` и `NaN`
- C: `20` и `63`
- D: `NaN` и `63`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

Заметь, что `diameter` это обычная функция, в то время как `perimeter` это стрелочная функция.

У стрелочных функций значение `this` указывает на окружающую область видимости, в отличие от обычных функций! Это значит, что при вызове `perimeter` значение `this` у этой функции указывает не на объект `shape`, а на внешнюю область видимости (например, window).

У этого объекта нет ключа `radius`, поэтому возвращается `undefined`.

</p>
</details>

---

###### 4. Что будет в консоли?

```javascript
+true;
!"Lydia";
```

- A: `1` и `false`
- B: `false` и `NaN`
- C: `false` и `false`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

Унарный плюс приводит операнд к числу. `true` это `1`, а `false` это `0`.

Строка `'Lydia'` это "истинное" значение. На самом деле мы спрашиваем "является ли это истинное значение ложным"? Ответ: `false`.

</p>
</details>

---

###### 5. Что НЕ является валидным?

```javascript
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

- A: `mouse.bird.size`
- B: `mouse[bird.size]`
- C: `mouse[bird["size"]]`
- D: Все варианты валидны

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

В JavaScript все ключи объекта являются строками (кроме Symbol). И хотя мы не _набираем_ их как строки, они всегда преобразовываются к строкам под капотом.

JavaScript интерпретирует (или распаковывает) операторы. При использовании квадратных скобок JS замечает `[` и продолжает пока не встретит `]`. Только после этого он вычислит то, что находится внутри скобок.

`mouse[bird.size]`: Сперва определяется `bird.size`, которое равно `"small"`. `mouse["small"]` возвращает `true`.

Но с записью через точку так не происходит. У `mouse` нет ключа `bird`. Таким образом, `mouse.bird` равно `undefined`. Затем мы запрашиваем ключ `size`, используя точечную нотацию: `mouse.bird.size`. Так как `mouse.bird` это `undefined`, мы запрашиваем `undefined.size`. Это не является валидным, и мы получаем ошибку типа `Cannot read property "size" of undefined`.

</p>
</details>

---

---

###### 6. Что будет в консоли?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- A: `Hello`
- B: `undefined`
- C: `ReferenceError`
- D: `TypeError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

В JavaScript все объекты являются _ссылочными_ типами данных.

Сперва переменная `c` указывает на объект. Затем мы указываем переменной `d` ссылаться на тот же объект, что и `c`.

<img src="https://i.imgur.com/ko5k0fs.png" width="200">

Когда ты изменяешь один объект, то изменяются значения всех ссылок, указывающих на этот объект.

</p>
</details>

---

###### 7. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

`new Number()` это встроенный конструктор функции. И хотя он выглядит как число, это не настоящее число: у него есть ряд дополнительных фич и это объект.

Оператор `==` разрешает приведение типов, он проверяет равенство _значений_. Оба значения равны `3`, поэтому возвращается `true`.

При использвании оператора `===` значение _и_ тип должны быть одинаковыми. Но в нашем случае это не так: `new Number()` это не число, это **объект**. Оба возвращают `false`.

</p>
</details>

---

###### 8. Каким будет результат?

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
freddie.colorChange("orange");
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: D

Функция `colorChange` является статичной. Статичные методы не имеют доступа к экземплярам класса. Так как `freddie` это экземпляр, то статичный метод там не доступен. Поэтому выбрасывается ошибка `TypeError`.

</p>
</details>

---

###### 9. Что будет в консоли?

```javascript
let greeting;
greetign = {}; // Опечатка!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

В консоли выведется объект, потому что мы только что создали пустой объект в глобальном объекте! Когда мы вместо `greeting` написали `greetign`, интерпретатор JS на самом деле выполнил `global.greetign = {}` (или `window.greetign = {}` в браузере).

Нужно использовать `"use strict"`, чтобы избежать такого поведения. Эта запись поможет быть уверенным в том, что переменная была определена перед тем как ей присвоили значение.

</p>
</details>

---

###### 10. Что произойдет?

```javascript
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- A: Ничего, всё в порядке!
- B: `SyntaxError`. Нельзя добавлять свойства функциям таким способом.
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

В JavaScript это возможно, т.к. функции это объекты! (Всё есть объект кроме примитивов).

Функция это специальный тип объекта, который можно вызвать. Функция это объект со свойствами.
A function is a special type of object. The code you write yourself isn't the actual function. The function is an object with properties. This property is invocable.

</p>
</details>

---

###### 11. Что будет в консоли?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

Нельзя добавлять свойства конструктору, как обычному объекту. Если нужно добавить фичу всем объектам, то необходимо использовать прототипы. В данном случае

```js
Person.prototype.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}
```

сделает метод `member.getFullName()` рабочим. В чем тут преимущество? Предположим, что мы добавили этот метод к конструктору. Возможно, не каждому экземпляру `Person` нужен этот метод. Это приведет к большим потерям памяти, т.к. все экземпляры будут иметь это свойство. Напротив, если мы добавим этот метод только к прототипу, у нас будет только одно место в памяти, к которому смогут обращаться все экземпляры!

</p>
</details>

---

###### 12. Что будет в консоли?

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

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` и `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` и `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` и `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` и `ReferenceError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

Для `sarah` мы не использовали ключевое слово `new`. Использование `new` приводит к созданию нового объекта. Но без `new` он указывает на **глобальный объект**!

Мы указали, что `this.firstName` равно `"Sarah"` и `this.lastName` равно `"Smith"`. На самом деле мы определили `global.firstName = 'Sarah'` и `global.lastName = 'Smith'`. `sarah` осталась `undefined`.

</p>
</details>

---

###### 13. Назовите три фазы распространения событий

- A: Цель > Захват > Всплытие
- B: Всплытие > Цель > Захват
- C: Цель > Всплытие > Захват
- D: Захват > Цель > Всплытие

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: D

Во время фазы **захвата** событие распространяется с элементов родителей до элемента цели. После достижения **цели** начинается фаза **всплытия**.

<img src="https://i.imgur.com/N18oRgd.png" width="200">

</p>
</details>

---

###### 14. Все объекты имют прототипы

- A: Да
- B: Нет

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

Все объекты имеют прототипы, кроме **базового объекта**. Базовый объект имеет доступ до некоторых методов и свойств, таких как `.toString`. Именно поэтому мы можем использовать встроенные методы JavaScript! Все эти методы доступны в прототипе. Если JavaScript не может найти метод непосредственно у объекта, он продолжает поиск по цепочке прототипов пока не найдет.

</p>
</details>

---

###### 15. Каким будет результат?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

JavaScript это **динамически типизированный язык**: мы не определяем тип переменных. Переменные могут автоматически быть преобразованы из одного типа в другой без нашего участия, что называется _неявным приведением типов_. **Приведение** это преобразование из одного типа в другой.

В этом примере JavaScript сконвертировал число `1` в строку, чтобы операция внутри функции имела смысл и вернула значение. Во время сложения числа (`1`) и строки (`'2'`) число преобразовывается к строке. Мы можем конкатенировать строки вот так: `"Hello" + "World"`. Таким образом, `"1" + "2"` возвращает `"12"`.

</p>
</details>

---

###### 16. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

**Постфиксный** унарный оператор `++`:

1. Возвращает значение (`0`)
2. Инкрементирует значение (теперь число равно `1`)

**Префиксный** унарный оператор `++`:

1. Инкрементирует значение (число теперь равно `2`)
2. Возвращает значение (`2`)

Результат: `0 2 2`.

</p>
</details>

---

###### 17. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

При использовании тегированных шаблонных литералов первым аргументом всегда будет массив строковых значений. Оставшимися аргументами будут значения переданных выражений!

</p>
</details>

---

###### 18. Что будет в консоли?

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("Ты взрослый!");
  } else if (data == { age: 18 }) {
    console.log("Ты все еще взрослый.");
  } else {
    console.log(`Хмм.. Кажется, у тебя нет возраста.`);
  }
}

checkAge({ age: 18 });
```

- A: `Ты взрослый!`
- B: `Ты все еще взрослый.`
- C: `Хмм.. Кажется, у тебя нет возраста.`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

В операциях сравнения примитивы сравниваются по их _значениям_, а объекты по _ссылкам_. JavaScript проверяет, чтобы объекты указывали на одну и ту же область памяти.

Сравниваемые объекты в нашем примере не такие: объект, переданный в качестве параметра, указывает на другую область памяти, чем объекты, используемые в сравнениях.

Поэтому `{ age: 18 } === { age: 18 }` и `{ age: 18 } == { age: 18 }` возвращают `false`.

</p>
</details>

---

###### 19. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Оператор распространения (`...args`) возвращает массив с аргументами. Массив это объект, поэтому `typeof args` возвращает `"object"`.

</p>
</details>

---

###### 20. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Используя `"use strict"`, можно быть уверенным, что мы по ошибке не побъявим глобальные переменные. Мы ранее нигде не объявляли переменную `age`, поэтому с использованием `"use strict"` возникнет `ReferenceError`. Без использования `"use strict"` ошибки не возникнет, а переменная `age` добавится в глобальный объект.

</p>
</details>

---

###### 21. Чему будет равно `sum`?

```javascript
const sum = eval("10*10+5");
```

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

`eval` выполняет код, переданный в виде строки. Если это выражение (как в данном случае), то вычисляется выражение. Выражение `10 * 10 + 5` вернет число `105`.

</p>
</details>

---

###### 22. Как долго будет доступен cool_secret?

```javascript
sessionStorage.setItem("cool_secret", 123);
```

- A: Всегда, данные не потеряются.
- B: Пока пользователь не закроет вкладку.
- C: Пока пользователь не закроет браузер, а не только вкладку.
- D: Пока пользователь не выключит компьютер.

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

Данные, сохраненные в `sessionStorage` очищаются после закрытия _вкладки_.

При использовании `localStorage` данные сохраняются навсегда. Очистить их можно, например, используя `localStorage.clear()`.

</p>
</details>

---

###### 23. Что будет в консоли?

```javascript
var num = 8;
var num = 10;

console.log(num);
```

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

С помощью ключевого слова `var` можно определять сколько угодно переменных с одним и тем же именем. Переменная будет хранить последнее присвоенное значение.

Но такой трюк нельзя проделать с `let` и `const`, т.к. у них блочная область видимости.

</p>
</details>

---

###### 24. Каким будет результат?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Все ключи объектов (кроме Symbols) являются строками, даже если заданы не в виде строк. Поэтому `obj.hasOwnProperty('1')` так же возвращает true.

Но это не работает для `set`. Значения `'1'` нет в `set`: `set.has('1')` возвращает `false`. Но `set.has(1)` вернет `true`.

</p>
</details>

---

###### 25. Что будет в консоли?

```javascript
const obj = { a: "one", b: "two", a: "three" };
console.log(obj);
```

- A: `{ a: "one", b: "two" }`
- B: `{ b: "two", a: "three" }`
- C: `{ a: "three", b: "two" }`
- D: `SyntaxError`

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Если есть два ключа с одинаковым именем, то ключ будет перезаписан. Его позиция сохранится, но значением будет последнее указанное.

</p>
</details>

---

###### 26. Глобальный контекст исполнения создает две вещи: глобальный объект и `this`

- A: Да
- B: Нет
- C: Это зависит

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

Базовый контекст исполнения это глобальный контекст исполнения: это то, что доступно где угодно в твоем коде.

</p>
</details>

---

###### 27. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: C

Оператор `continue` пропускает итерацию, если условие возвращает `true`.

</p>
</details>

---

###### 28. Каким будет результат?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: A

`String` это встроенный конструктор, к которому можно добавлять свойства. Я добавила метод к его прототипу. Строки-примитивы автоматически конвертируются к строкам-объектам. Поэтому все строки (строковые объекты) имеют доступ к этому методу!

</p>
</details>

---

###### 29. Что будет в консоли?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

Ключи объекта автоматически конвертируются в строки. Мы собираемся добавить объект в качестве ключа к объекту `a` со значением `123`.

Тем не менее, когда мы приводим объект к строке, он становится `"[object Object]"`. Таким образом, мы говорим, что `a["Object object"] = 123`. Потом мы делаем то же самое. `c` это другой объект, который мы неявно приводим к строке. Поэтому `a["Object object"] = 456`.

Затем, когда мы выводим `a[b]`, мы имеем в виду `a["Object object"]`. Мы только что установили туда значение `456`, поэтому в результате получаем `456`.

</p>
</details>

---

###### 30. Каким будет результат?

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

<details><summary><b>Ответ</b></summary>
<p>

#### Ответ: B

Мы вызываем функцию `setTimeout` первой. Тем не менее, она выводится в консоль последней

Это происходит из-за того, что в браузерах у нас есть не только рантайм движок, но и `WebAPI`. `WebAPI` предоставляет нам функцию `setTimeout` и много других возможностей. Например, DOM.

После того как _коллбек_ отправлен в `WebAPI`, функция `setTimeout` (но не коллбек!) вынимается из стека.

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

Теперь вызывается `foo`, и `"First"` выводится в консоль.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo` достается из стека, и вызывается `baz`. `"Third"` выводится в консоль.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

WebAPI не может добавлять содержимое в стек когда захочет. Вместо этого он отправляет коллбек-функцию в так называемую _очередь_.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

Здесь на сцену выходит цикл событий (event loop). **Event loop** проверяет стек и очередь задач. Если стек пустой, то он берет первый элемент из очереди и отправляет его в стек.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

Вызывается `bar`, в консоль выводится `"Second"` и эта функция достается из стека.

</p>
</details>

---

###### 31. What is the event.target when clicking the button?

```html
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```

- A: Outer `div`
- B: Inner `div`
- C: `button`
- D: An array of all nested elements.

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

The deepest nested element that caused the event is the target of the event. You can stop bubbling by `event.stopPropagation`

</p>
</details>

---

###### 32. When you click the paragraph, what's the logged output?

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

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

If we click `p`, we see two logs: `p` and `div`. During event propagation, there are 3 phases: capturing, target, and bubbling. By default, event handlers are executed in the bubbling phase (unless you set `useCapture` to `true`). It goes from the deepest nested element outwards.

</p>
</details>

---

###### 33. What's the output?

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

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

With both, we can pass the object to which we want the `this` keyword to refer to. However, `.call` is also _executed immediately_!

`.bind.` returns a _copy_ of the function, but with a bound context! It is not executed immediately.

</p>
</details>

---

###### 34. What's the output?

```javascript
function sayHi() {
  return (() => 0)();
}

typeof sayHi();
```

- A: `"object"`
- B: `"number"`
- C: `"function"`
- D: `"undefined"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

The `sayHi` function returns the returned value of the immediately invoked function (IIFE). This function returned `0`, which is type `"number"`.

FYI: there are only 7 built-in types: `null`, `undefined`, `boolean`, `number`, `string`, `object`, and `symbol`. `"function"` is not a type, since functions are objects, it's of type `"object"`.

</p>
</details>

---

###### 35. Which of these values are falsy?

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
- D: All of them are falsy

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

There are only six falsy values:

- `undefined`
- `null`
- `NaN`
- `0`
- `''` (empty string)
- `false`

Function constructors, like `new Number` and `new Boolean` are truthy.

</p>
</details>

---

###### 36. What's the output?

```javascript
console.log(typeof typeof 1);
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`typeof 1` returns `"number"`.
`typeof "number"` returns `"string"`

</p>
</details>

---

###### 37. What's the output?

```javascript
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

When you set a value to an element in an array that exceeds the length of the array, JavaScript creates something called "empty slots". These actually have the value of `undefined`, but you will see something like:

`[1, 2, 3, 7 x empty, 11]`

depending on where you run it (it's different for every browser, node, etc.)

</p>
</details>

---

###### 38. What's the output?

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

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `catch` block receives the argument `x`. This is not the same `x` as the variable when we pass arguments. This variable `x` is block-scoped.

Later, we set this block-scoped variable equal to `1`, and set the value of the variable `y`. Now, we log the block-scoped variable `x`, which is equal to `1`.

Outside of the `catch` block, `x` is still `undefined`, and `y` is `2`. When we want to `console.log(x)` outside of the `catch` block, it returns `undefined`, and `y` returns `2`.

</p>
</details>

---

###### 39. Everything in JavaScript is either a...

- A: primitive or object
- B: function or object
- C: trick question! only objects
- D: number or object

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

JavaScript only has primitive types and objects.

Primitive types are `boolean`, `null`, `undefined`, `bigint`, `number`, `string`, and `symbol`.

What differentiates a primitive from an object is that primitives do not have any properties or methods; however, you'll note that `'foo'.toUpperCase()` evaluates to `'FOO'` and does not result in a `TypeError`. This is because when you try to access a property or method on a primitive like a string, JavaScript will implicity wrap the object using one of the wrapper classes, i.e. `String`, and then immediately discard the wrapper after the expression evaluates. All primitives except for `null` and `undefined` exhibit this behaviour.

</p>
</details>

---

###### 40. What's the output?

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

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`[1, 2]` is our initial value. This is the value we start with, and the value of the very first `acc`. During the first round, `acc` is `[1,2]`, and `cur` is `[0, 1]`. We concatenate them, which results in `[1, 2, 0, 1]`.

Then, `[1, 2, 0, 1]` is `acc` and `[2, 3]` is `cur`. We concatenate them, and get `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---

###### 41. What's the output?

```javascript
!!null;
!!"";
!!1;
```

- A: `false` `true` `false`
- B: `false` `false` `true`
- C: `false` `true` `true`
- D: `true` `true` `false`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

`null` is falsy. `!null` returns `true`. `!true` returns `false`.

`""` is falsy. `!""` returns `true`. `!true` returns `false`.

`1` is truthy. `!1` returns `false`. `!false` returns `true`.

</p>
</details>

---

###### 42. What does the `setInterval` method return?

```javascript
setInterval(() => console.log("Hi"), 1000);
```

- A: a unique id
- B: the amount of milliseconds specified
- C: the passed function
- D: `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

It returns a unique id. This id can be used to clear that interval with the `clearInterval()` function.

</p>
</details>

---

###### 43. What does this return?

```javascript
[..."Lydia"];
```

- A: `["L", "y", "d", "i", "a"]`
- B: `["Lydia"]`
- C: `[[], "Lydia"]`
- D: `[["L", "y", "d", "i", "a"]]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

A string is an iterable. The spread operator maps every character of an iterable to one element.

</p>
</details>
