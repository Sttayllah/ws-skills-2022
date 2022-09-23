# Langage Javascript

> ❌ A travailler

> ✔️ Auto validation par l'étudiant

## 🎓 J'ai compris et je peux expliquer

- les `structures` de base du langage ✔️

### Variables && constantes

-[x]Types: number, string, boolean, null, undefined, object(array, function, class, object) -[x]Déclaration -[x]Portée

### Opérateurs

-[x]Calculs: +, -, _, %, / -[x]Affectations: =, +=, -=, /=, _=, %=, ++, -- -[x]Comparaisons: <, >, <=, >=, ==, === -[x]Logiques: &&, ||,! -[x]Décompostion: ...

### Structures conditionnelles

-[x]Conditions:If, if...else, if...else if -[x]Boucles: while, for, do...while, for...of, switch...case -[x]Operateurs conditionnels: ternaire (?...:), ET logique (&&)

### Functions

-[x]Déclaration -[x]Paramètres && arguments -[x]Portée

### Functions natives

-[x]Générales: parseInt(), tostring().. -[x]Array: map(), push(), join(), sort(), filter()... -[x]String: toLowercase(), to Uppercase(), slice(), substr(), split()... -[x]Calculs: parseInt(), parseFloat()...

### Méthodes d'objets

-[x]Array: Array.isArray(), Array.from() -[x]Object: , Object.entries(), Object.keys(), Object.entries() -[x]Set: Set(), add(), has(), delete()

- les normes `ecmascript` ❌

- l'utilisation de l'`asynchrone` ✔️

> Les fonctions asynchrones s'executent en parallèle des fonctions synchrones et permettent à ces dernières de continuer de s'executer. Les fonctions asynchrones renvoient des Promises qui permettent de "chainer des fonctions asynchrones. Les promises sont des objets représentant l'état de l'opération(en cours, terminée, echec), elles permettent de manipuler le résultat futur.

- les spécifités du mot-clef `this` ❌ / ✔️

## 💻 Je code en Javascript

### Un exemple de code commenté ❌ / ✔️

```javascript
const wildersMap = (dataFromApi) => {
  const newData = dataFromApi.map((wilder) => {
    const wilderSkills = wilder.grades.map((grade) => {
      return { title: grade.skill.name, votes: grade.grade };
    });
    return { name: wilder.name, id: wilder.id, skills: wilderSkills };
  });
  return newData;
};
```

### Utilisation dans un projet ✔️

[lien github](https://github.com/Sttayllah/node-live-coding/blob/main/src/controller/wilder.js)

Description :

### J'ai utilisé ce langage en production ❌

[lien du projet](...)

Description :

### J'ai utilisé ce langage en environement professionnel ❌

Description :

## 🌐 J'utilise des ressources

### Titre

- lien
- description

## 🚧 Je franchis les obstacles

### Point de blocage ❌ / ✔️

Description:

Plan d'action : (à valider par le formateur)

- action 1 ❌ / ✔️
- action 2 ❌ / ✔️
- ...

Résolution :

## 📽️ J'en fais la démonstration

- J'ai ecrit un [tutoriel](...) ❌ / ✔️
- J'ai fait une [présentation](...) ❌ / ✔️
