# Liste de questions JavaScript (Avancée)

Je poste quotidiennement des questions à choix multiple sur mon [Instagram](https://www.instagram.com/theavocoder), que je posterai aussi ici !

Des basiques au avancée: testez vos connaissances en JavaScript, rafraichissez vos connaissances, ou préparez vous pour un entretien de code ! :muscle: :rocket: Je mets à jour ce dépôt chaque semaine avec des nouvelles questions.

Les réponses se trouvent dans les sections fermées en dessous des questions, cliquez simplement dessus pour les faire apparaitre. Bonne chance :heart:

[English](./README.md)
[中文版本](./README-zh_CN.md)
[Русский](./README_ru-RU.md)
[Western Balkan](./README-bs.md)

---

###### 1. Quelle est la sortie ?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "Lydia";
  let age = 21;
}

sayHi();
```

- A: `Lydia` et `undefined`
- B: `Lydia` et `ReferenceError`
- C: `ReferenceError` et `21`
- D: `undefined` et `ReferenceError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: D

Dans la fonction, nous déclarons en premier la variable `name` grâce au mot clé `var`. Cela signifie que la variable est "levée" _(hoisted)_ (l'espace mémoire est définie à la phase de création) avec pour valeur par défaut `undefined`, jusqu'à ce que le script atteigne la ligne de définition de la variable. Nous n'avons pas encore défini la variable lorsque nous essayons d'afficher la variable `name`, donc elle a toujours la valeur `undefined`.

Les variables avec le mot clé `let` (et `const`) sont "levées" _(hoisted)_, mais contrairement à `var`, elle n'est pas <i>initialisée</i>. Elles ne sont pas accessible avant la ligne qui les declare (initialise). C'est appelé la "zone morte temporaire". Lorsque nous essayons d'accéder aux variables avant leur déclaration, JavaScript renvoit une `ReferenceError`.

</p>
</details>

---

###### 2. Quelle est la sortie ?

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1);
}
```

- A: `0 1 2` et `0 1 2`
- B: `0 1 2` et `3 3 3`
- C: `3 3 3` et `0 1 2`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

À cause du système de queue dans JavaScript, la fonction de rappel _(callback)_ du `setTimeout` est appellée _après_ que la boucle soit exécutée. Comme la variable `i` dans la première boucle est déclarée avec le mot clé `var`, c'est une variable global. Pendant la boucle, nous incrémentons la valeur de `i` par `1` à chaque fois, en utilisant l'opérateur arithmétique `++`. Lorsque la fonction de rappel _(callback)_ du `setTimeout` est invoquée, `i` est égal à `3` dans le premier exemple.

Dans la seconde boucle, la variable `i` est déclarée avec le mot clé `let` : les variables déclarées avec `let` (et `const`) ont une portée de bloc (tout ce qui est entre `{ }` est considéré comme un bloc). Pendant chaque itération, `i` aura une nouvelle valeur, et chaque valeur sera définie dans la boucle.

</p>
</details>

---

###### 3. Quelle est la sortie ?

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

- A: `20` et `62.83185307179586`
- B: `20` et `NaN`
- C: `20` et `63`
- D: `NaN` et `63`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

Notez que le valeur de `diameter` est une fonction régulière, alors que `perimeter` est une fonction fléchée.

Avec les fonctions fléchée, le mot clé `this` réfère à son périmètre actuel, contrairement au fonctions régulières ! Cela signifie que lorsque nous appelons `perimeter`, elle ne réfère pas à l'objet shape, mais à son périmètre actuel (`window` par exemple).

Il n'y a pas de valeur `radius` dans cet objet, qui retournera `undefined`.

</p>
</details>

---

###### 4. Quelle est la sortie ?

```javascript
+true;
!"Lydia";
```

- A: `1` et `false`
- B: `false` et `NaN`
- C: `false` et `false`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

L'opérateur arithmétique `+` essait de convertir un opérande en une valeur numérique. `true` devient `1`, et `false` devient `0`.

La chaine de caractère `'Lydia'` est une value considérée comme vraie _(truthy)_. Ce que nous sommes actuellement entrain de demander, c'est "est-ce que cette valeur vraie est fausse ?". Ce qui retournera `false`.

</p>
</details>

---

###### 5. Laquelle est vraie ?

```javascript
const bird = {
  size: "small"
};

const mouse = {
  name: "Mickey",
  small: true
};
```

- A: `mouse.bird.size` n'est pas valide
- B: `mouse[bird.size]` n'est pas valide
- C: `mouse[bird["size"]]` n'est pas valide
- D: Toutes sont valides

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

En JavaScript, toutes les clé d'objet sont des chaines de caractères (sauf si c'est un Symbol). Bien que nous ne puission pas les _typer_ comme des chaines de caractères, elles sont converties en chaines de caractères sous le capot.

JavaScript interprète (ou décompresse) les instructions. Lorsque nous utilisons la notation pas crochet, il voit le premier crochet `[` et continue jusqu'à ce qu'il trouve le crochet fermant `]`. Seulement après, il évalue l'instruction.

`mouse[bird.size]` : Premièrement, il évalue `bird.size`, qui est `"small"`. `mouse["small"]` retourne `true`.

Cependant, avec la notation par points, cela n'arrive pas. `mouse` n'a pas de clé appelée `bird`, ce qui signifie que `mouse.bird` est `undefined`. Puis, on demande `size` en utilisant la notation par point : `mouse.bird.size`. Comme `mouse.bird` est `undefined`, on demande `undefined.size`. Cela n'est pas valide, et nous aurons une erreur similaire à `Impossible de lire la propriété "size" de undefined`.

</p>
</details>

---

---

###### 6. Quelle est la sortie ?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```

- A: `Hello`
- B: `Hey`
- C: `undefined`
- D: `ReferenceError`
- E: `TypeError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

En JavaScript, tous les object intéragisent par _référence_ en plaçant égaux les uns aux autres.

Premièrement, la variable `c` contaient une valeur d'objet. Plus tard, nous assignons `d` avec la même réference que `c` à l'objet.

<img src="https://i.imgur.com/ko5k0fs.png" width="200">

Quand on modifie un objet, on les modifie donc tous.

</p>
</details>

---

###### 7. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

`new Number()` est une fonction globale. Bien qu'il ressamble à un nombre, ce n'en est pas vraiment un : il a une poignée de fonctionnalités supplémentaire and est un objet.

Quand nous utilisons l'opérateur `==`, il vérifie seulement qu'il s'agisse de la même _valeur_. Les deux ont pour valeur `3`, donc il retourne `true`.

Cependant, quand on utilise l'opérateur `===`, les 2 valeurs _et_ type doivent être les même. `new Number()` n'est pas un nombre, c'est un **objet**, il retourne `false`.

</p>
</details>

---

###### 8. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: D

La fonction `colorChange` est statique. Les méthodes statiques sont désignée pour vivre seulement dans le constructeur qui les a créer et ne peuvent pas être transférer aux enfants. Comme `freddie` est un enfant, la fonction n'est pas transférée et non disponible dans l'instance de `freddie` : une erreur `TypeError` est renvoyée.

</p>
</details>

---

###### 9. Quelle est la sortie ?

```javascript
let greeting;
greetign = {}; // Typo!
console.log(greetign);
```

- A: `{}`
- B: `ReferenceError: greetign is not defined`
- C: `undefined`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

It logs the object, because we just created an empty object on the global object! When we mistyped `greeting` as `greetign`, the JS interpreter actually saw this as `global.greetign = {}` (or `window.greetign = {}` in a browser).

In order to avoid this, we can use `"use strict"`. This makes sure that you have declared a variable before setting it equal to anything.

</p>
</details>

---

###### 10. Que ce passe-t-il lorsque nous faisons ça ?

```javascript
function bark() {
  console.log("Woof!");
}

bark.animal = "dog";
```

- A: Rien, c'est tout à fait bon !
- B: `SyntaxError`. Vous ne pouvez pas ajouter de propriétés à une fonction de cette façon.
- C: `undefined`
- D: `ReferenceError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

This is possible in JavaScript, because functions are objects! (Everything besides primitive types are objects)

A function is a special type of object. The code you write yourself isn't the actual function. The function is an object with properties. This property is invocable.

</p>
</details>

---

###### 11. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

You can't add properties to a constructor like you can with regular objects. If you want to add a feature to all objects at once, you have to use the prototype instead. So in this case,

```js
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

would have made `member.getFullName()` work. Why is this beneficial? Say that we added this method to the constructor itself. Maybe not every `Person` instance needed this method. This would waste a lot of memory space, since they would still have that property, which takes of memory space for each instance. Instead, if we only add it to the prototype, we just have it at one spot in memory, yet they all have access to it!

</p>
</details>

---

###### 12. Quelle est la sortie ?

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

- A: `Person {firstName: "Lydia", lastName: "Hallie"}` et `undefined`
- B: `Person {firstName: "Lydia", lastName: "Hallie"}` et `Person {firstName: "Sarah", lastName: "Smith"}`
- C: `Person {firstName: "Lydia", lastName: "Hallie"}` et `{}`
- D:`Person {firstName: "Lydia", lastName: "Hallie"}` et `ReferenceError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

For `sarah`, we didn't use the `new` keyword. When using `new`, it refers to the new empty object we create. However, if you don't add `new` it refers to the **global object**!

We said that `this.firstName` equals `"Sarah"` and `this.lastName` equals `"Smith"`. What we actually did, is defining `global.firstName = 'Sarah'` and `global.lastName = 'Smith'`. `sarah` itself is left `undefined`.

</p>
</details>

---

###### 13. Quelle sont les trois phases de propagation des événements ?

- A: Target > Capturing > Bubbling
- B: Bubbling > Target > Capturing
- C: Target > Bubbling > Capturing
- D: Capturing > Target > Bubbling

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: D

Durant la phase de **capture** _(capturing)_, l'événement passe par les événements parent jusqu'à l'élément ciblé. Il attient ensuite l'élément **ciblé** _(target)_, et commence à **bouillonner** _(bubbling)_.

<img src="https://i.imgur.com/N18oRgd.png" width="200">

</p>
</details>

---

###### 14. Tous les objects ont des prototypes.

- A: true
- B: false

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

All objects have prototypes, except for the **base object**. The base object has access to some methods and properties, such as `.toString`. This is the reason why you can use built-in JavaScript methods! All of such methods are available on the prototype. Although JavaScript can't find it directly on your object, it goes down the prototype chain and finds it there, which makes it accessible for you.

</p>
</details>

---

###### 15. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

JavaScript is a **dynamically typed language**: we don't specify what types certain variables are. Values can automatically be converted into another type without you knowing, which is called _implicit type coercion_. **Coercion** is converting from one type into another.

In this example, JavaScript converts the number `1` into a string, in order for the function to make sense and return a value. During the addition of a numeric type (`1`) and a string type (`'2'`), the number is treated as a string. We can concatenate strings like `"Hello" + "World"`, so what's happening here is `"1" + "2"` which returns `"12"`.

</p>
</details>

---

###### 16. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

The **postfix** unary operator `++`:

1. Returns the value (this returns `0`)
2. Increments the value (number is now `1`)

The **prefix** unary operator `++`:

1. Increments the value (number is now `2`)
2. Returns the value (this returns `2`)

This returns `0 2 2`.

</p>
</details>

---

###### 17. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

If you use tagged template literals, the value of the first argument is always an array of the string values. The remaining arguments get the values of the passed expressions!

</p>
</details>

---

###### 18. Quelle est la sortie ?

```javascript
function checkAge(data) {
  if (data === { age: 18 }) {
    console.log("Vous êtes un adulte !");
  } else if (data == { age: 18 }) {
    console.log("Vous êtes toujours un adulte.");
  } else {
    console.log(`Hmm.. Vous n'avez pas l'âge, je suppose.`);
  }
}

checkAge({ age: 18 });
```

- A: `Vous êtes un adulte !`
- B: `Vous êtes toujours un adulte.`
- C: `Hmm.. Vous n'avez pas l'âge, je suppose.`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

When testing equality, primitives are compared by their _value_, while objects are compared by their _reference_. JavaScript checks if the objects have a reference to the same location in memory.

The two objects that we are comparing don't have that: the object we passed as a parameter refers to a different location in memory than the object we used in order to check equality.

This is why both `{ age: 18 } === { age: 18 }` and `{ age: 18 } == { age: 18 }` return `false`.

</p>
</details>

---

###### 19. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

The spread operator (`...args`.) returns an array with arguments. An array is an object, so `typeof args` returns `"object"`

</p>
</details>

---

###### 20. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

With `"use strict"`, you can make sure that you don't accidentally declare global variables. We never declared the variable `age`, and since we use `"use strict"`, it will throw a reference error. If we didn't use `"use strict"`, it would have worked, since the property `age` would have gotten added to the global object.

</p>
</details>

---

###### 21. Quelle est la valeur de `sum` ?

```javascript
const sum = eval("10*10+5");
```

- A: `105`
- B: `"105"`
- C: `TypeError`
- D: `"10*10+5"`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

`eval` evaluates codes that's passed as a string. If it's an expression, like in this case, it evaluates the expression. The expression is `10 * 10 + 5`. This returns the number `105`.

</p>
</details>

---

###### 22. Pendant combien de temps `cool_secret` sera-t'il accessible ?

```javascript
sessionStorage.setItem("cool_secret", 123);
```

- A: Pour toujours, les données ne seront pas perdues.
- B: Jusqu'à ce que l'utilisateur ferme l'onglet.
- C: Jusqu'à ce que l'utilisateur ferme son navigateur en entier, pas seulement son onglet.
- D: Jusqu'à ce que l'utilisateur éteindra son ordinateur.

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

The data stored in `sessionStorage` is removed after closing the _tab_.

If you used `localStorage`, the data would've been there forever, unless for example `localStorage.clear()` is invoked.

</p>
</details>

---

###### 23. Quelle est la sortie ?

```javascript
var num = 8;
var num = 10;

console.log(num);
```

- A: `8`
- B: `10`
- C: `SyntaxError`
- D: `ReferenceError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

With the `var` keyword, you can declare multiple variables with the same name. The variable will then hold the latest value.

You cannot do this with `let` or `const` since they're block-scoped.

</p>
</details>

---

###### 24. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

All object keys (excluding Symbols) are strings under the hood, even if you don't type it yourself as a string. This is why `obj.hasOwnProperty('1')` also returns true.

It doesn't work that way for a set. There is no `'1'` in our set: `set.has('1')` returns `false`. It has the numeric type `1`, `set.has(1)` returns `true`.

</p>
</details>

---

###### 25. Quelle est la sortie ?

```javascript
const obj = { a: "un", b: "deux", a: "trois" };
console.log(obj);
```

- A: `{ a: "un", b: "deux" }`
- B: `{ b: "deux", a: "trois" }`
- C: `{ a: "trois", b: "deux" }`
- D: `SyntaxError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

If you have two keys with the same name, the key will be replaced. It will still be in its first position, but with the last specified value.

</p>
</details>

---

###### 26. The JavaScript global execution context creates two things for you: the global object, and the "this" keyword.

- A: Vrai
- B: Faux
- C: Ça dépends

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

The base execution context is the global execution context: it's what's accessible everywhere in your code.

</p>
</details>

---

###### 27. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

The `continue` statement skips an iteration if a certain condition returns `true`.

</p>
</details>

---

###### 28. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

`String` is a built-in constructor, which we can add properties to. I just added a method to its prototype. Primitive strings are automatically converted into a string object, generated by the string prototype function. So, all strings (string objects) have access to that method!

</p>
</details>

---

###### 29. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

Object keys are automatically converted into strings. We are trying to set an object as a key to object `a`, with the value of `123`.

However, when we stringify an object, it becomes `"[Object object]"`. So what we are saying here, is that `a["Object object"] = 123`. Then, we can try to do the same again. `c` is another object that we are implicitly stringifying. So then, `a["Object object"] = 456`.

Then, we log `a[b]`, which is actually `a["Object object"]`. We just set that to `456`, so it returns `456`.

</p>
</details>

---

###### 30. Quelle est la sortie ?

```javascript
const foo = () => console.log("Premier");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Troisième");

bar();
foo();
baz();
```

- A: `Premier` `Second` `Troisième`
- B: `Premier` `Troisième` `Second`
- C: `Second` `Premier` `Troisième`
- D: `Second` `Troisième` `Premier`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

We have a `setTimeout` function and invoked it first. Yet, it was logged last.

This is because in browsers, we don't just have the runtime engine, we also have something called a `WebAPI`. The `WebAPI` gives us the `setTimeout` function to start with, and for example the DOM.

After the _callback_ is pushed to the WebAPI, the `setTimeout` function itself (but not the callback!) is popped off the stack.

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

Now, `foo` gets invoked, and `"First"` is being logged.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo` is popped off the stack, and `baz` gets invoked. `"Third"` gets logged.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

The WebAPI can't just add stuff to the stack whenever it's ready. Instead, it pushes the callback function to something called the _queue_.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

This is where an event loop starts to work. An **event loop** looks at the stack and task queue. If the stack is empty, it takes the first thing on the queue and pushes it onto the stack.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

`bar` gets invoked, `"Second"` gets logged, and it's popped off the stack.

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

If we click `p`, we see two logs: `p` and `div`. During event propagation, there are 3 phases: capturing, target, and bubbling. By default, event handlers are executed in the bubbling phase (unless you set `useCapture` to `true`). It goes from the deepest nested element outwards.

</p>
</details>

---

###### 33. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: D

With both, we can pass the object to which we want the `this` keyword to refer to. However, `.call` is also _executed immediately_!

`.bind.` returns a _copy_ of the function, but with a bound context! It is not executed immediately.

</p>
</details>

---

###### 34. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

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

###### 36. Quelle est la sortie ?

```javascript
console.log(typeof typeof 1);
```

- A: `"number"`
- B: `"string"`
- C: `"object"`
- D: `"undefined"`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

`typeof 1` returns `"number"`.
`typeof "number"` returns `"string"`

</p>
</details>

---

###### 37. Quelle est la sortie ?

```javascript
const numbers = [1, 2, 3];
numbers[10] = 11;
console.log(numbers);
```

- A: `[1, 2, 3, 7 x null, 11]`
- B: `[1, 2, 3, 11]`
- C: `[1, 2, 3, 7 x empty, 11]`
- D: `SyntaxError`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

When you set a value to an element in an array that exceeds the length of the array, JavaScript creates something called "empty slots". These actually have the value of `undefined`, but you will see something like:

`[1, 2, 3, 7 x empty, 11]`

depending on where you run it (it's different for every browser, node, etc.)

</p>
</details>

---

###### 38. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

JavaScript only has primitive types and objects.

Primitive types are `boolean`, `null`, `undefined`, `bigint`, `number`, `string`, and `symbol`.

What differentiates a primitive from an object is that primitives do not have any properties or methods; however, you'll note that `'foo'.toUpperCase()` evaluates to `'FOO'` and does not result in a `TypeError`. This is because when you try to access a property or method on a primitive like a string, JavaScript will implicity wrap the object using one of the wrapper classes, i.e. `String`, and then immediately discard the wrapper after the expression evaluates. All primitives except for `null` and `undefined` exhibit this behaviour.

</p>
</details>

---

###### 40. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

`[1, 2]` is our initial value. This is the value we start with, and the value of the very first `acc`. During the first round, `acc` is `[1,2]`, and `cur` is `[0, 1]`. We concatenate them, which results in `[1, 2, 0, 1]`.

Then, `[1, 2, 0, 1]` is `acc` and `[2, 3]` is `cur`. We concatenate them, and get `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---

###### 41. Quelle est la sortie ?

```javascript
!!null;
!!"";
!!1;
```

- A: `false` `true` `false`
- B: `false` `false` `true`
- C: `false` `true` `true`
- D: `true` `true` `false`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

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

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

A string is an iterable. The spread operator maps every character of an iterable to one element.

</p>
</details>
