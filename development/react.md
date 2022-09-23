# Titre de la compétence

> ❌ A travailler

> ✔️ Auto validation par l'étudiant

## 🎓 J'ai compris et je peux expliquer

- l'état (_state_) pour contrôler l'affichage d'un composant ✔️
- les composants enfants et les _props_ qu'on leur passe ✔️
- le déclenchement d'instructions en fonction des actions de l'utilisateur ✔️
- le déclenchement d'instructions en fonction de l'étape du cycle de vie du composant ou du changement de valeur de ses props ✔️
- l'usage d'un reducer (_useReducer_) pour gérer un état composé dans un composant
- l'état stocké dans un composant avec un _context provider_ et accessible dans ses descendants via `useContext` ✔️

## 💻 J'utilise

### Un exemple personnel commenté ✔️

Exemple UseState, props, déclenchement conditionnel:

```Typescript
import React, { useState } from 'react';
import './App.scss';
import Button from 'react-bootstrap/Button';
import Form from 'react-bootstrap/Form';
import { collection, addDoc, getFirestore } from 'firebase/firestore';
import { Argonaut, COLLECTION } from './firebase';
import { firebaseApp } from './config';
import { Section } from './Component/Section';
import Callout from './Component/Callout';
import { CalloutImage } from './Component/CalloutImage';
import { ToastContainer, toast } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
import { ArgonautList } from './Component/ArgonautList';

const App = () => {
  const db = getFirestore(firebaseApp);
  const [toggleAnimation, setToggleAnimation] = useState<boolean>(true);
  const [showForm, setShowForm] = useState<boolean>(true);
  const [showSheep, setShowSheep] = useState<boolean>(true);
  const [showEyes, setShowEyes] = useState<boolean>(false);
  //switch from 2 columns to 3
  const [threeColumns, setThreeColumns] = useState<boolean>(false);
  const [argonautName, setArgonautName] = useState<string>('');
  //to get collection size
  const [size, setSize] = useState<number>(0);
  const [salmonBgImage, setSalmonBgImage] = useState<number>(0);
  // increment salmonBgImage to toggle between image
  const _handleSalmonBgImage = () => {
    setSalmonBgImage(salmonBgImage + 1);
    setShowEyes(true);
    setToggleAnimation(false);
    salmonBgImage === 1 && setThreeColumns(true);
  };
  // switch for animations & button bg color when hover
  const _handleAnimations = () => {
    setToggleAnimation((previousState) => !previousState);
  };
  // switch to hide form & eyes
  const _handleShowForm = () => {
    setShowForm((previousState) => !previousState);
    setShowEyes(false);
  };

  // add an argonaut
  const _handleAddArgonaut = async () => {
    const argonaut: Argonaut = {
      displayName: argonautName,
    };

    const docRef = await addDoc(collection(db, COLLECTION.Argonauts), argonaut);
    console.log('Document written with ID: ', docRef.id);
  };

  const rest = (49 - size).toString();
  //show toast when an argonaut is added
  const _notify = () => {
    toast('Plus que ' + rest + ' places..!!', {
      position: 'top-center',
      autoClose: 2000,
      hideProgressBar: false,
      closeOnClick: true,
      pauseOnHover: true,
      draggable: true,
      progress: undefined,
      theme: 'dark',
    });
  };
  // reset input
  const _handleReset = (e: any):void => {
    e.preventDefault();
    e.target.reset();
    setArgonautName('');
  };
  // switch between shepp image/hide sheep after 200ms
  const _handleSheep = (e: any) => {
    e.target.src = 'sheep2.png';
    setTimeout(() => {
      e.target.style.display = 'none';
      setShowSheep(false);
    }, 400);
  };

  const FullButton =
    // handle adding argonauts  until they reach 20
    size < 20 ? (
      <Button
        type="submit"
        className={!toggleAnimation ? 'button' : 'btnBis'}
        variant="btn-button"
        onClick={async () => {
          // show spinner
          await _handleAddArgonaut();
          _notify();
          // hide spinner
        }}
        onMouseEnter={_handleAnimations}
        onMouseLeave={_handleAnimations}
      >
        Ajouter un argonaute
      </Button>
    ) : //on click show galere1
    salmonBgImage === 0 ? (
      <Button
        type="submit"
        className={!toggleAnimation ? 'button' : 'btnBis'}
        variant="btn-button"
        onClick={_handleSalmonBgImage}
        onMouseEnter={_handleAnimations}
        onMouseLeave={_handleAnimations}
      >
        Allons peler ce mouton..!!
      </Button>
    ) : //on click show galere2/argonauts on 3 columns & sheep eyes
    salmonBgImage === 1 ? (
      <Button
        type="submit"
        className={!toggleAnimation ? 'button' : 'btnBis'}
        variant="btn-button"
        onClick={_handleSalmonBgImage}
        onMouseEnter={_handleAnimations}
        onMouseLeave={_handleAnimations}
      >
        On avance à rien dans ce canoë
      </Button>
    ) : (
      // on click hide form & show sheep
      <Button
        type="submit"
        className={!toggleAnimation ? 'button' : 'btnBis'}
        variant="btn-button"
        onClick={_handleShowForm}
        onMouseEnter={_handleAnimations}
        onMouseLeave={_handleAnimations}
      >
        Allons peler ce mouton...!!!!²
      </Button>
    );

  const CalloutForm =
    // show input until argonauts reach 20
    size < 20 ? (
      <Callout>
        <div className="callout-title">VOGUE LA GALÈRE</div>
        <div className="callout-text">
          <p className="p">Pierre?................Présent!</p>
          <p className="p">Pierre?............................Présent!</p>
          <p className="p">Pierre?..........Présent!</p>
          <p className="p">
            Ceux qui ont besoin d'aller aux toilettes, c'est maintenant!
          </p>
        </div>
        <Form className="form" onSubmit={_handleReset}>
          <Form.Group className=" inputGroup mb-3">
            <Form.Control
              className="input"
              type="text"
              placeholder="Entrez le nom de l'argonaute"
              onChange={(e) => {
                // console.log('onChange :', e);
                const newValue = e.target.value;
                setArgonautName(newValue);
                // console.log('newValue :', newValue);
              }}
            />
          </Form.Group>
          <div>
            {FullButton}
            <ToastContainer
              position="top-center"
              autoClose={4000}
              hideProgressBar={false}
              newestOnTop={false}
              closeOnClick
              rtl={false}
              pauseOnFocusLoss
              draggable
              pauseOnHover
            />
          </div>
        </Form>
      </Callout>
    ) : //if showForm show form Callout without input
    showForm ? (
      <Callout>
        {salmonBgImage === 0 ? (
          <>
            <div className="callout-title">
              OH CAPITAINE, MON CAPITAINE..??!!
            </div>
            <div className="callout-text">
              <p className="p">.....48</p>
              <p className="p">.....49</p>
              <p className="p">.....20. Le compte est bon!!</p>
              <p className="p">
                Je compte sur vous pour le strict respect du protocole!!!
              </p>
            </div>
          </>
        ) : salmonBgImage === 1 ? (
          <>
            <div className="callout-title">NOM DE ZEUS...!!!</div>
            <div className="callout-text">
              <p className="p">Le protocole...!!!!</p>
              <p className="p">
                On avait dit on alterne, un gros, un petit, un gros, un petit...
              </p>
              <p className="p1">
                ...Suis pas gros, je suis juste facile à voir....
              </p>
              <p className="p">Nestor, Orphée, Pollux, A l'arrière...!!!</p>
            </div>
          </>
        ) : (
          <>
            <div className="callout-title">C'EST UN FAMEUX TROIS...</div>
            <div className="callout-text">
              <p className="pEnd">ET ON LUI PÈLERA LE JONC......</p>
            </div>
          </>
        )}

        <Form className="form" onSubmit={_handleReset}>
          {FullButton}
        </Form>
      </Callout>
    ) : (
      //else hide form Callout
      <></>
    );
  const CalloutImageSheep =
    // show sleep images until argonauts reach 20
    size < 20 ? (
      <>
        <CalloutImage imageSrc="sleep1.png" />
        <CalloutImage imageSrc="sleep2.png" />
        <CalloutImage imageSrc="sleep3.png" />
        <CalloutImage imageSrc="sleep4.png" />
      </>
    ) : // if salmonBGImage =0 show bubble & question mark images
    salmonBgImage === 0 ? (
      <>
        <CalloutImage move={toggleAnimation} imageSrc="questionMark.png" />
        <CalloutImage move={toggleAnimation} imageSrc="bubble1.png" />
        <CalloutImage move={toggleAnimation} imageSrc="bubble2.png" />
      </>
    ) : // if salmonBgImage=1 show only 1 bubble image
    salmonBgImage === 1 ? (
      <>
        <CalloutImage move={toggleAnimation} imageSrc="questionMark.png" />
        <CalloutImage move={toggleAnimation} imageSrc="bubble2.png" />
      </>
    ) : (
      <>
        {showEyes && (
          <CalloutImage
            move={toggleAnimation}
            imageSrc={toggleAnimation ? 'eyes.png' : 'eyes2.png'}
          />
        )}
        {showEyes && (
          <CalloutImage move={toggleAnimation} imageSrc="surprise1.png" />
        )}
        {showEyes && (
          <CalloutImage move={toggleAnimation} imageSrc="surprise2.png" />
        )}
        {showEyes && (
          <CalloutImage move={toggleAnimation} imageSrc="surprise3.png" />
        )}
        {/* show sheep1 */}
        {!showForm && showSheep && (
          <>
            {/* show sheep2 on mouse enter */}
            <CalloutImage onMouseEnter={_handleSheep} imageSrc="sheep1.png" />
            <CalloutImage imageSrc="surprise1.png" />
            <CalloutImage imageSrc="surprise2.png" />
            <CalloutImage imageSrc="surprise3.png" />
          </>
        )}
        {/* hide sheep2 && show run away image when !showSheep  */}
        {!showSheep && <CalloutImage imageSrc="runAway.png" />}
      </>
    );
  return (
    <div className="App">
      <Section backgroundColor="white">
        <Section
          backgroundColor="salmon"
          backgroundImage={'galere' + salmonBgImage + '.png'}
        >
          <ArgonautList columns={threeColumns} size={setSize} />
        </Section>
        {CalloutImageSheep}
        {CalloutForm}
      </Section>
    </div>
  );
};

export default App;

```

Exemple UseEffect:

```Typescript
  //get collection
  useEffect(() => {
    const q = query(
      collection(db, COLLECTION.Argonauts),
      orderBy('displayName', 'asc')
      //   where('disabled', '!=', true)
    );

    const unsubscribe = onSnapshot(q, (querySnapshot) => {
      const newArgonauts: Argonaut[] = [];
      // console.log(querySnapshot.size);
      props.size(querySnapshot.size);
      querySnapshot.forEach((doc) => {
        const data = doc.data();
        const argonaut: Argonaut = {
          displayName: data.displayName,
        };
        newArgonauts.push(argonaut);
      });
      setArgonauts(newArgonauts);
    });
    return unsubscribe;
  }, []);
```

### Utilisation dans un projet ❌ / ✔️

[lien github](...)

Description :

### Utilisation en production si applicable❌ / ✔️

[lien du projet](https://poc-wcs.web.app/)

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
