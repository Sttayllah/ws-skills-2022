# Titre de la compétence

> ❌ A travailler

> ✔️ Auto validation par l'étudiant

## 🎓 J'ai compris et je peux expliquer

- Comment développer en utilisant un système de _livereloading_ (`nodemon` par exemple) ✔️

> Nodemon est un outil permettant de surveiller les modifications de fichier dans le répertoire et donc de redémarrer le processus.

Installation "environnement dev":

```
npm install --save-dev nodemon
```

Création du script:

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon index.js"
  },
```

- La connexion de mon application à une base de données avec et sans ORM/ODM (avec `mongodb` puis `mongoose` par exemple) ✔️
  > SQLite est une librairie qui propose un moteur de BDD relationnelles SQL directement intégré à une application.

Installation:

```
npm install sqlite3
```

> TyOrm est une librairie ORM (Object Relationnal Mapping) éxécutée dans Node, écrite en Typescript qui permet de faciliter la manipulation des données d'une BDD traduites sous forme d'objet tout en tirant parti de l'architecture MVC.

Installation:

```
npm install typeorm
```

- Le développement d'une API REST et GraphQL (avec les packages `express` et `graphql` par exemple) ❌ / ✔️
- _Bonus : la manipulation des fichiers système avec `fs` et l'utilisation des streams en NodeJS_ ❌ / ✔️

## 💻 J'utilise

### Un exemple personnel commenté ✔️

Entité Wilder:

```Typescript
import { Column, Entity, OneToMany, PrimaryGeneratedColumn } from "typeorm";
import Grade from "./grade";

@Entity()
class Wilder {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @OneToMany(() => Grade, (grade) => grade.wilder, {
    onDelete: "CASCADE",
    eager: true,
  })
  grade: Grade[];
}
export default Wilder;
```

Création de la BDD depuis les entités:

```Typescript
import { DataSource } from "typeorm";
import Wilder from "./entity/wilder";
import Skill from "./entity/skill";
import Grade from "./entity/grade";

const dataSource = new DataSource({
  type: "sqlite",
  database: "wildersdb.sqlite",
  synchronize: true,
  entities: [Wilder, Skill, Grade],
  logging: ["query", "error"],
});
export default dataSource;
```

Requête depuis le WilderController:

```Typescript
getOne: async (req, res) => {
    const id: string = req.params.wilderId;
    try {
      const wilder = await dataSource.getRepository(Wilder).findOne({
        where: { id: parseInt(id) },
        relations: {
          grade: {
            skill: true,
          },
        },
      });

      if (wilder !== null) {
        const { id, name, grade } = wilder;
        const formatedData = {
          id,
          name,
          skill: grade.map((e) => {
            return {
              id: e.skill.id,
              name: e.skill.name,
              rate: e.grade,
            };
          }),
        };
        res.send(formatedData);
      }
    } catch (e) {
      console.log(e);
      res.send(req.body);
    }
  },
```

### Utilisation dans un projet ❌ / ✔️

[lien github](...)

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
