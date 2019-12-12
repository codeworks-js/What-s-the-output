
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