# TypeScript

> ❌ A travailler

> ✔️ Auto validation par l'étudiant

## 🎓 J'ai compris et je peux expliquer

- l'intéret de TypeScript dans l'IDE ✔️

-[x]Auto-complétion, refactoring, suggestions, lisibilité -[x]Détection des problèmes avant l'exécution

- les types de bases ✔️

-[x]Types: number, string, boolean, null, undefined, array, void, union type, conditional, generic -[x]Déclaration

> Exemple de déclaration:

```typescript
const isTrue: boolean = false;

const Function = (a: number | null, b?: string): void => {
  //something here....
};
```

- comment et pourquoi étendre une interface ✔️

> Les interfaces définissent la structure d'une classe, d'une fonction ou d'un objet. Elle contient des champs, des propriétés ou des méthodes qui peuvent être obligatoires, facultatifs(?) ou en lecture seule(readOnly). Les interfaces peuvent s'étendre mutuellement ce qui permet de les diviser en composants réutilisables.

- les classes et les decorators ❌ / ✔️

## 💻 J'utilise

### Un exemple personnel commenté ✔️

Exemple de déclaration d'une interface:

```typescript
interface Component {
  key: string;
  name: string;
  placeholder?: string;
  label: string;
  type: ComponentType;
  customMetadata: CustomMeta[];
  doNotPublish?: boolean;
  editableByPatient: boolean;
  requiredToSave: boolean;
  requiredToLock: boolean;
}
```

Exemple d'extension d'une interface:

```typescript
interface Video extends Component {
  type: ComponentType.VIDEO;
  url: string;
}
```

> Le composant "Video" prend toutes les propriétés du composant "Component" et y ajoute les siennes.

### Utilisation dans un projet ❌ / ✔️

[lien github](...)
//TODO

Description :

### Utilisation en production si applicable❌ / ✔️

[lien du projet](...)

Description :

### Utilisation en environement professionnel ❌ / ✔️

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
