
###### 1. Quelle est la sortie ?

```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = "CodeWorks";
  let age = 2;
}

sayHi();
```

<details><summary><b>Réponse</b></summary>
<p>

Dans la fonction, nous déclarons en premier la variable `name` grâce au mot clé `var`. Cela signifie que la variable est "levée" _(hoisted)_ (l'espace mémoire est définie à la phase de création) avec pour valeur par défaut `undefined`, jusqu'à ce que le script atteigne la ligne de définition de la variable. Nous n'avons pas encore défini la variable lorsque nous essayons d'afficher la variable `name`, donc elle a toujours la valeur `undefined`.

Les variables avec le mot clé `let` (et `const`) sont "levées" _(hoisted)_, mais contrairement à `var`, elle n'est pas <i>initialisée</i>. Elles ne sont pas accessible avant la ligne qui les déclare (initialise). C'est appelé la "zone morte temporaire". Lorsque nous essayons d'accéder aux variables avant leur déclaration, JavaScript renvoie une `ReferenceError`.

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

<details><summary><b>Réponse</b></summary>
<p>

À cause du système de queue dans JavaScript, la fonction de rappel _(callback)_ du `setTimeout` est appelée _après_ que la boucle soit exécutée. Comme la variable `i` dans la première boucle est déclarée avec le mot-clé `var`, c'est une variable globale. Pendant la boucle, nous incrémentons la valeur de `i` de `1` à chaque fois, en utilisant l'opérateur arithmétique `++`. Lorsque la fonction de rappel _(callback)_ du `setTimeout` est invoquée, `i` est égal à `3` dans le premier exemple.

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


<details><summary><b>Réponse</b></summary>
<p>


Notez que la valeur de `diameter` est une fonction régulière, alors que celle de `perimeter` est une fonction fléchée.

Avec les fonctions fléchée, le mot clé `this` réfère à son périmètre actuel, contrairement aux fonctions régulières ! Cela signifie que lorsque nous appelons `perimeter`, elle ne réfère pas à l'objet `shape`, mais à son périmètre actuel (`window` par exemple).

Il n'y a pas de valeur `radius` dans cet objet, on retournera `undefined`.

</p>
</details>

---

###### 4. Laquelle est vraie ?

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

#### Réponse : A

En JavaScript, toutes les clés d'objet sont des chaînes de caractères (sauf si c'est un Symbol). Bien que nous ne puissions pas les _typer_ comme des chaînes de caractères, elles sont converties en chaînes de caractères sous le capot.

JavaScript interprète (ou décompresse) les instructions. Lorsque nous utilisons la notation pas crochet, il voit le premier crochet `[` et continue jusqu'à ce qu'il trouve le crochet fermant `]`. Seulement après, il évalue l'instruction.

`mouse[bird.size]` : Premièrement, il évalue `bird.size`, qui est `"small"`. `mouse["small"]` retourne `true`.

Cependant, avec la notation par points, cela n'arrive pas. `mouse` n'a pas de clé appelée `bird`, ce qui signifie que `mouse.bird` est `undefined`. Puis, on demande `size` en utilisant la notation par point : `mouse.bird.size`. Comme `mouse.bird` est `undefined`, on demande `undefined.size`. Cela n'est pas valide, et nous aurons une erreur similaire à `Impossible de lire la propriété "size" de undefined`.

</p>
</details>

---


###### 5. Quelle est la sortie ?

```javascript
let c = { greeting: "Hey!" };
let d;

d = c;
c.greeting = "Hello";
console.log(d.greeting);
```


<details><summary><b>Réponse</b></summary>
<p>


En JavaScript, tous les objets interagissent par _référence_ lorsqu'on les définit égaux les uns aux autres.

Premièrement, la variable `c` contaient une valeur d'objet. Plus tard, nous assignons `d` avec la même référence que `c` à l'objet.

<img src="https://i.imgur.com/ko5k0fs.png" width="200">

Quand on modifie un objet, on les modifie donc tous.

</p>
</details>

---

###### 6. Quelle est la sortie ?

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "vert" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "rouge" });
freddie.colorChange("orange");
```

<details><summary><b>Réponse</b></summary>
<p>


La fonction `colorChange` est statique. Les méthodes statiques sont désignées pour vivre seulement dans le constructeur qui les a créé et ne peuvent pas être transférer aux enfants. Comme `freddie` est un enfant, la fonction n'est pas transférée et n'est pas disponible dans l'instance de `freddie` : une erreur `TypeError` est renvoyée.

</p>
</details>

---

###### 7. Quelle est la sortie ?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const matthieu = new Person("Matthieu", "Albert");
const malek = Person("Malek", "Labiadh");

console.log(matthieu);
console.log(malek);
```


<details><summary><b>Réponse</b></summary>
<p>


Pour `malek`, nous n'avons pas utilisé le mot clé `new`. Quand nous utilisons `new`, il fait référence à un nouvel objet vide que nous créons. Cependant, nous n'ajoutons pas `new`. Il réfère à **l'objet global** !

Nous disons que `this.firstName` est égal à `"Malek"` et que `this.lastName` est égal à `Labiadh`. Ce que nous faisons c'est définir `global.firstName = 'Malek'` et `global.lastName = 'Labiadh'`. La variable `malek` elle-même reste à `undefined`. Le pauvre ...

</p>
</details>

---

###### 8. Quelle est la sortie ?

```javascript
function sum(a, b) {
  return a + b;
}

sum(1, "2");
```

<details><summary><b>Réponse</b></summary>
<p>


JavaScript est un **langage à types dynamiques** : nous n'avons pas besoin de spécifier le types des variables. Les valeurs peuvent être automatiquement converties vers les autres types sans que vous le sachiez, c'est ce que l'on appelle _la conversion de types implicites_ _(implicit type coercion)_.

Dans cette exemple, JavaScript convertit le nombre `1` en une chaîne de caractère, afin que la fonction ait du sens et puisse renvoyer une valeur. Durant l'addition d'un type numérique (`1`) et d'un type chaîne de caractère (`'2'`), le nombre est traité comme une chaîne de caractère. Nous pouvons concaténer les chaînes de caractères comme `"Hello" + "World"`, c'est donc ce qui arrive ici avec `"1" + "2"` qui retourne `"12"`.

</p>
</details>

---

###### 9. Quelle est la sortie ?

```javascript
let number = 0;
console.log(number++);
console.log(++number);
console.log(number);
```

<details><summary><b>Réponse</b></summary>
<p>


L'opérateur arithmétique **postfix** `++` :

1. Retourne la valeur (ici il retourne `0`)
2. Incrémente la valeur (le nombre est maintenant égal à `1`)

L'opérateur arithmétique **préfix** `++` :

1. Incrémente la valeur (le nombre est maintenant égal à `2`)
2. Retourne la valeur (ici il retourne `2`)

Cela retourne donc `0 2 2`.

</p>
</details>

---


###### 10. Quelle est la sortie ?

```javascript
function checkSkills(data) {
  if (data === { skill: 50 }) {
    console.log("Vous êtes un bon joueur ! GG");
  } else if (data == { skill: 50 }) {
    console.log("Vous êtes toujours un bon joueur.");
  } else {
    console.log(`Hmm.. Vous n'avez pas la moyenne, il faut qu'on bosse!!`);
  }
}

checkSkills({ skill: 50 });
```


<details><summary><b>Réponse</b></summary>
<p>


Lorsque l'on teste une égalité, les primitifs sont comparés par leur valeur, alors que les objets sont comparés par leur _référence_. JavaScript vérifie si les objets ont une référence à la même zone de la mémoire.=

Les 2 objets que nous comparons n'ont pas ça : l'objet passé en paramètre fait référence à une zone mémoire différente que l'objet que nous utilisons pour faire la comparaison.

C'est pourquoi les 2 conditions `{ skill: 50 } === { skill: 50 }` et `{ skill: 50 } == { skill: 50 }` retournent `false`.

</p>
</details>

---


###### 11. Quelle est la sortie ?

```javascript
for (let i = 1; i < 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
```

<details><summary><b>Réponse</b></summary>
<p>


L'instruction `continue` ignore une itération si une condition donnée renvoie `true`.
Et donc la sortie est : `1` `2` `4`

</p>
</details>

---


###### 12. Quelle est la sortie ?

```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```


<details><summary><b>Réponse</b></summary>
<p>


Les clés d'objet sont automatiquement converties en chaînes de caractères. Nous essayons de définir un objet en tant que clé de l'objet `a`, avec la valeur `123`.

Cependant, lorsque nous transformons un objet en chaîne de caractère, il devient `"[Objet objet]"`. Donc, ce que nous disons ici, c'est que un `a["Objet objet"] = 123`. Ensuite, nous pouvons essayer de refaire la même chose. `c` est un autre objet que nous sommes implicitement en train de transformer en chaîne de caractère. Donc, `a["Objet objet"] = 456`.

Ensuite, nous affichons `a[b]`, qui est en fait `a["Objet objet"]`. Que nous venons de définir à `456`, nous renvoyons donc `456`.

</p>
</details>

---


###### 13. Quelle est la sortie ?

```javascript
const foo = () => console.log("Premier");
const bar = () => setTimeout(() => console.log("Second"));
const baz = () => console.log("Troisième");

bar();
foo();
baz();
```

<details><summary><b>Réponse</b></summary>
<p>


Nous avons une fonction `setTimeout` et nous l'avons d'abord appelée. Pourtant, il a été affiché en dernier.

En effet, dans les navigateurs, nous n’avons pas seulement le moteur d’exécution, nous avons aussi quelque chose appelé `WebAPI`. `WebAPI` nous donne la fonction` setTimeout` pour commencer, et par exemple le DOM.

Une fois que la fonction de rappel _(callback)_ est poussée via la WebAPI, la fonction `setTimeout` elle-même (mais pas la fonction de rappel !) est extraite de la pile.

<img src="https://i.imgur.com/X5wsHOg.png" width="200">

Maintenant, `foo` est invoqué et `"Premier"` est affiché.

<img src="https://i.imgur.com/Pvc0dGq.png" width="200">

`foo` est extrait de la pile et `baz` est invoqué. `"Troisième"` est affiché.

<img src="https://i.imgur.com/WhA2bCP.png" width="200">

WebAPI ne peut simplement pas ajouter des éléments à la pile dès qu’elle est prête. Au lieu de cela, elle pousse la fonction de rappel vers quelque chose appelé la _file d'attente_.

<img src="https://i.imgur.com/NSnDZmU.png" width="200">

C'est ici qu'une boucle d'événement commence à fonctionner. La **boucle d'événement** examine la pile et la file d'attente des tâches. Si la pile est vide, il prend la première chose dans la file d'attente et la pousse sur la pile.

<img src="https://i.imgur.com/uyiScAI.png" width="200">

`bar` est invoqué, `"Second"` est affiché et il est sorti de la pile.

</p>
</details>

---

###### 14. Quelle est la sortie ?

```javascript
const person = { name: "Edwin" };

function sayHi(age) {
  console.log(`${this.name} a ${age} ans`);
}

sayHi.call(person, 28);
sayHi.bind(person, 28);
```

<details><summary><b>Réponse</b></summary>
<p>


Avec les deux, nous pouvons transmettre l'objet auquel nous voulons que le mot clé `this` fasse référence. Cependant, `.call` est aussi _exécuté immédiatement_ !

`.bind.` renvoie une copie de la fonction, mais avec un contexte lié ! Elle n'est pas exécutée immédiatement.
Et donc la sortie est: `Edwin a 28 ans` `function`

</p>
</details>

---
###### 15. Quelle est la sortie ?

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

<details><summary><b>Réponse</b></summary>
<p>


Le bloc `catch` reçoit l'argument `x`. Ce n'est pas le même `x` que la variable que nous passons en arguments. Cette variable `x` a une portée de bloc.

Plus tard, nous définissons cette variable de bloc égale à `1` et définissons la valeur de la variable `y`. Maintenant, nous affichons la variable `x` de portée de bloc, dont la valeur est égale à `1`.

En dehors du bloc `catch`, `x` est toujours `undefined` et `y` est égal à `2`. Lorsque nous voulons `console.log(x)` en dehors du bloc `catch`, il renvoie `undefined`, et `y` renvoie `2` et donc la sortie est: `1` `undefined` `2`

</p>
</details>

---


###### 16. Quelle est la sortie ?

```javascript
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse : C

`[1, 2]` est notre valeur initiale. C'est la valeur avec laquelle nous commençons et la valeur du tout premier `acc`. Au premier tour, `acc` est `[1,2]` et `cur` est `[0, 1]`. Nous les concaténons, ce qui donne `[1, 2, 0, 1]`.

Ensuite, `acc` est `[1, 2, 0, 1]` et `cur` est `[2, 3]`. Nous les concaténons et obtenons `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---

###### 17. Que retourne ceci ?

```javascript
[..."Bangaly"];
```

<details><summary><b>Réponse</b></summary>
<p>


Une chaîne de caractère est itérable. L'opérateur de déconstruction transforme chaque caractère d'un itérable en un élément.
La sortie est donc : `["B", "a", "n", "g", "a", "l", "y"]`

</p>
</details>

---

###### 18. Quelle est la sortie ?

```javascript
let person = { name: "Nourdine" };
const members = [person];
person = null;

console.log(members);
```

<details><summary><b>Réponse</b></summary>
<p>


Tout d'abord, nous déclarons une variable `person` avec la valeur d'un objet possédant une propriété `name`.

<img src="https://i.imgur.com/TML1MbS.png" width="200">

Ensuite, nous déclarons une variable appelée `membres`. Nous définissons le premier élément de ce tableau égal à la valeur de la variable `person`. Les objets interagissent par référence quand ils sont égaux. Lorsque vous affectez une référence d'une variable à une autre, vous créez une copie de cette référence. (notez qu'ils n'ont pas la même référence !)

<img src="https://i.imgur.com/FSG5K3F.png" width="300">

Ensuite, nous définissons la variable `person` égale à `null`.

<img src="https://i.imgur.com/sYjcsMT.png" width="300">

Nous modifions seulement la valeur de la variable `person`, et non le premier élément du tableau, car cet élément a une référence (copiée) différente de l'objet. Le premier élément de `members` conserve sa référence à l'objet d'origine. Lorsque nous affichons le tableau `members`, le premier élément contient toujours la valeur de l'objet, qui est affiché.
la sortie est : `[{ name: "Nourdine" }]`

</p>
</details>

---


###### 19. Quelle est la sortie ?

```javascript
const settings = {
  username: "idrissa",
  level: 19,
  health: 90
};

const data = JSON.stringify(settings, ["level", "health"]);
console.log(data);
```


<details><summary><b>Réponse</b></summary>
<p>


Le second argument de `JSON.stringify` est le _replaçant_. Le remplaçant peut être une fonction ou un tableau, et vous permet de contrôler quoi et comment les valeurs doivent être stringifiées.

Si le remplaçant est un _tableau_, seules les propriétés dont les noms sont inclus dans le tableau seront ajoutées à la chaîne JSON. Dans ce cas, seules les propriétés avec les noms `"level"` et `"health"` sont incluses, `"username"` est exclu. `data` est maintenant égal à `"{"level":19, "health":90}"`.

Si le remplaçant est une _fonction_, cette fonction est appelée sur chaque propriété de l'objet que vous personnalisez. La valeur renvoyée par cette fonction sera la valeur de la propriété lorsqu'elle sera ajoutée à la chaîne JSON. Si la valeur est `undefined`, cette propriété est exclue de la chaîne JSON.

</p>
</details>

---

###### 20. Quelle est la sortie ?

```javascript
const user = { name: "Pierrick", age: 28 };
const admin = { admin: true, ...user };

console.log(admin);
```

<details><summary><b>Réponse</b></summary>
<p>


Il est possible de combiner des objets en utilisant l'opérateur de déconstruction `...`. Il vous permet de créer des copies des paires clé / valeur d'un objet et de les ajouter à un autre objet. Dans ce cas, nous créons des copies de l'objet `user` et nous les ajoutons à l'objet` admin`. L'objet `admin` contient maintenant les paires clé / valeur copiées, ce qui donne `{admin: true, name: "Pierrick", age: 28}`.

</p>
</details>

---
###### 21. Quelle est la sortie ?

```javascript
const myPromise = () => Promise.resolve('Je suis résolue')

function firstFunction() {
  myPromise().then(res => console.log(res))
  console.log('second')
}

async function secondFunction() {
  console.log(await myPromise())
  console.log('second')
}

firstFunction()
secondFunction()
```

<details><summary><b>Réponse</b></summary>
<p>


`second`, `Je suis résolue` et `Je suis résolue`, `second`

</p>
</details>

---

###### 22. Quelle est la sortie ?

```javascript
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age }

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list))
console.log(getUser(user))
```

- A: `[1, [2, 3, 4]]` and `undefined`
- B: `[1, [2, 3, 4]]` and `{ name: "Lydia", age: 21 }`
- C: `[1, 2, 3, 4]` and `{ name: "Lydia", age: 21 }`
- D: `Error` and `{ name: "Lydia", age: 21 }`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: A

La fonction `getList` reçoit un tableau en argument. On peut voir ce qui est en paramètre comme:

 `[x, ... y] = [1, 2, 3, 4]`

 Avec le `... y`, nous plaçons tous les arguments" restants "dans un tableau. Dans ce cas, les arguments restants sont `2`, `3` et `4`. La valeur de `y` est un tableau contenant tous ces paramètres.
 La valeur de `x` est égale à `1`, donc lorsque nous enregistrons `[x, y]`, `[1, [2, 3, 4]]` est enregistré.

 La fonction `getUser` reçoit un objet. Avec les arrow functions, nous n'avons pas besoin d'écrire des crochets si nous retournons juste une valeur. Donc, si on souhaite renvoyer un _objet_ à partir d'une arrow function, on doit l'écrire entre parenthèses, sinon aucune valeur n'est renvoyée! La fonction suivante aurait renvoyé un objet:

`` `const getUser = user => ({name: user.name, age: user.age})` ``

Puisqu'aucune valeur n'est retournée dans ce cas, la fonction retourne `undefined`.
Donc l'output est: `[1, [2, 3, 4]]` et `undefined`

</p>
</details>

---

###### 23. Quelle est la sortie ?

```javascript
const name = "CodeWorker"

console.log(name())
```

- A: `SyntaxError`
- B: `ReferenceError`
- C: `TypeError`
- D: `undefined`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

</p>
</details>

---

###### 24. Quelles sont les methodes qui modifient l'array original?

```javascript
const emojis = ['✨', '🥑', '😍']

emojis.map(x => x + '✨')
emojis.filter(x => x !== '🥑')
emojis.find(x => x !== '🥑')
emojis.reduce((acc, cur) => acc + '✨')
emojis.slice(1, 2, '✨') 
emojis.splice(1, 2, '✨')
```

- A: `All of them`
- B: `map` `reduce` `slice` `splice`
- C: `map` `slice` `splice` 
- D: `splice`

<details><summary><b>Répose</b></summary>
<p>

#### Répose: D

Avec la méthode `splice`, nous modifions le tableau d'origine en supprimant, remplaçant ou ajoutant des éléments. Dans ce cas, nous avons supprimé 2 éléments de l'index 1 (nous avons supprimé `'🥑'` et `'😍'`) et ajouté à la place l'émoji ✨.

`map`,` filter` et `slice` renvoient un nouveau tableau,` find` renvoie un élément et `reduce` renvoie une valeur réduite.

</p>
</details>

---

###### 25. What will happen?

```javascript
let config = {
  alert: setInterval(() => {
    console.log('Alert!')
  }, 1000)
}

config = null
```

- A: The `setInterval` callback won't be invoked
- B: The `setInterval` callback gets invoked once
- C: The `setInterval` callback will still be called every second
- D: We never invoked `config.alert()`, config is `null`

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

Normally when we set objects equal to `null`, those objects get _garbage collected_ as there is no reference anymore to that object. However, since the callback function within `setInterval` is an arrow function (thus bound to the `config` object), the callback function still holds a reference to the `config` object. As long as there is a reference, the object won't get garbage collected. Since it's not garbage collected, the `setInterval` callback function will still get invoked every 1000ms (1s).

</p>
</details>

---

###### 26. Quelle est la sortie ?

```javascript
const person = {
  name: "Matthieu",
  age: 28
}

const changeAge = (x = { ...person }) => x.age += 1
const changeAgeAndName = (x = { ...person }) => {
  x.age += 1
  x.name = "Sarah"
}

changeAge(person)
changeAgeAndName()

console.log(person)
```


<details><summary><b>Réponse</b></summary>
<p>

`{ name: "Matthieu", age: 29 }`.

</p>
</details>

---

###### 26. Quelle est la sortie ?

```javascript
const person = {
	firstName: "Luffy",
	lastName: "Monkey D.",
	friend: {
		name: "Zoro",
		description : "Best swords man in the world"
	},
	getFullName() {
		return `${this.firstName} ${this.lastName}`;
	}
};

console.log(person.friend?.name);
console.log(person.friend?.family?.name);
console.log(person.getFullName?.());
console.log(member.getLastName?.());
```


<details><summary><b>Réponse</b></summary>
<p>

`Zoro` `undefined` `Luffy Monkey D.` `undefined`

</p>
</details>

---

###### 27. Quelle est la sortie ?

```javascript
const myFunc = ({ x, y, z }) => {
	console.log(x, y, z);
};

myFunc(1, 2, 3);
```

<details><summary><b>Réponse</b></summary>
<p>

`undefined` `undefined` `undefined`

</p>
</details>

---

###### 28. Quelle est la sortie ?

```javascript
const emojis = ["🥑", ["✨", "✨", ["🍕", "🍕"]]];

console.log(emojis.flat(1));
```

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: B

`['🥑', '✨', '✨', ['🍕', '🍕']]`

</p>
</details>

---

###### 29. Quelle est la sortie ?

```javascript
function Ship() {
  this.make = "POLAR TANG";
  return { make: "Thousand Sunny" };
}

const myShip = new Ship();
console.log(myShip.make);
```


<details><summary><b>Réponse</b></summary>
<p>

Lorsque vous renvoyez une propriété, la valeur de la propriété est égale à la valeur routourné et non à la valeur définie dans le constructeur. Nous renvoyons la chaîne `" Thousand Sunny "`, donc `myShip.make` est égal à `"Thousand Sunny" `.

</p>
</details>
---

###### 30. Quelle est la sortie ?

```javascript
const numbers = [1, 2, 3, 4, 5];
const [y] = numbers;

console.log(y);
```

<details><summary><b>Réponse</b></summary>
<p>

#### Réponse: C

Nous pouvons décompresser les valeurs des tableaux ou les propriétés des objets grâce à la déstructuration.
Par exemple:
```javascript
[a, b] = [1, 2];
```

<img src="https://i.imgur.com/ADFpVop.png" width="200">

La valeur de `a` est désormais `1` et la valeur de `b` est désormais `2`. Ce que nous avons réellement fait dans la question, c'est:

```javascript
[y] = [1, 2, 3, 4, 5];
```

<img src="https://i.imgur.com/NzGkMNk.png" width="200">

Cela signifie que la valeur de `y` est égale à la première valeur du tableau, qui est le nombre `1`. Lorsque nous enregistrons `y`, `1` est renvoyé.

</p>
</details>

---