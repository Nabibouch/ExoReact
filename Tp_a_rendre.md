#  Exercice – Explication de code et utilisation des Hooks

## Partie 1 – Compréhension du code

Voici un code que vous devez **analyser et expliquer**, en vous appuyant sur le cours et la documentation de React.

---

```html
<!DOCTYPE html>
<html lang="fr">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>React – useEffect & useState</title>

  <!-- React, ReactDOM et Babel -->
  <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <!-- TailwindCSS -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>

<body class="bg-slate-900 text-white min-h-screen flex flex-col items-center justify-center p-4">
  <h1 class="text-3xl font-bold mb-6">Suivi de température</h1>
  <div id="root" class="w-full max-w-xl"></div>

  <script type="text/babel">
    const { useState, useEffect } = React; //On récupère les méthodes de l'objet React pour nous faciliter l'écriture

    function Counter() {
      const [count, setCount] = useState(0); // Ici on crée un état ou state pour sauvegarder la variable count et pouvoir la modifier dans le composant 

      useEffect(() => {
        console.log("MONTAGE / MISE À JOUR :", count);

        // Fonction de nettoyage (appelée au démontage)
        return () => {
          console.log("NETTOYAGE :", count);
        };
      });

      return (
        //React.fragment sert ici à envelopper les différentes balise car react ne peut retourner qu'un seul élément parent, on aurait pu mettre une div mais cela est moins propre
        <React.Fragment>
        // on affiche le count actuelle, on le met entre {} car il s'agit d'un state en Js. Pour écrire du js "pure" on utilise les {}//
          <p>Compteur : {count}</p>
          <button
            onClick={() => setCount(count + 1)} //On incrémente de 1 count à chaque clique
            className="bg-blue-600 hover:bg-blue-700 px-4 py-2 rounded mt-2"
          >
            +1
          </button>
        </React.Fragment>
      );
    }

    function App() {
      const [status, setStatus] = useState(true);

      return (
        <div className="bg-slate-800 p-4 rounded-xl shadow-lg">
          <button
            onClick={() => setStatus(!status)} //lorsque l'on clique, status se change en sa valeur inverse (true à false ou false à true)
            className="bg-gray-600 hover:bg-gray-700 px-4 py-2 rounded mb-4"
          >
            Changer le statut
          </button>

          {status && <Counter />} //On affiche le composant seulement si status est true
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById("root")).render(<App />);
  </script>
</body>
</html>
```

---

### ✏️ Travail demandé

1. **Expliquez le fonctionnement du code** :

   * Quel est le rôle de `useState` dans `Counter` et `App` ?

Dans counter, le useState permet d'avoir une variable (un état) modifiable dans le composant. Ici, il permet de sauvegarder la valeur du count et de le mettre à jour. Aussi, lorsque le count est modifié, le composant va se mettre à jour pour afficher la nouvelle valeur de count (il va se rerender)

Dans App, il sert à sauvegarder la valeur de status (true ou false) et donc comme avant pouvoir être mise à jour. La principal utilité ici est d'afficher ou non le counter en fonction de la valeur de satus

   * À quoi sert `useEffect` ici ?

Ici, le useEffect permet simplement d'afficher dans la console : "MONTAGE / MISE À JOUR :" suivi de la valeur de count (Exemple si count = 4 : MONTAGE / MISE À JOUR : 4) lorsque l'application se monte ou se met à jour mais aussi d'afficher dans cette même console : "NETTOYAGE :" suivi de la valeur actuelle de count.

   * Quand la fonction de nettoyage est-elle appelée ?

La fonction est appelé lors du démontage de l'application (lorsque le composant est retiré du DOM)

   * Que se passe-t-il quand on clique sur "Changer le statut" ?

Lorsque l'on appuie sur changer le statut, on utilise la fonction comprise dans le onClick (() => setStatus(!status)) qui change la valeur de status par son contraire (de true à false ou de false à true)

   * Quelle est la différence entre le **montage**, la **mise à jour**, et le **démontage** du composant `Counter` ?

Le montage correspond au moment ou react ajoute un composant au DOM
La mise à jour lorsqu'un composant est rerender (quand un état est changé par exemple)
Et le démontage correspond au moment ou le composant est retiré du DOM

2. Appuyez-vous sur les concepts vus en cours :

   * Le cycle de vie d'un composant fonctionnel.
   * Le comportement de `useEffect` à chaque rendu.

Je ne pense pas que ce soit une question ou bien je ne l'ai pas comprise 
---

## Partie 2 – Exemple d'utilisation de `useReducer`

Donnez un **exemple simple** d'un composant React utilisant le hook `useReducer` pour gérer un compteur, `useReducer` est un super `useState`
Vous pouvez vous inspirer de ce modèle :

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "incremente" : 
      return {...state, count : state.count + 1}
    case "decremente" :
      return {...state, count : state.count - 1}

    case "reset" : 
      return {...state, count : 0}
    // TODO 
    default:
      return state;
  }
}

export default function CounterReducer() {

  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div className="p-4 bg-slate-800 rounded-lg text-white">
    <h1>Counter avec reducer : {state.count}</h1>
    <button onClick = {() => dispatch({type : "incremente"})}>incremente</button>
    <button onClick = {() => dispatch({type : "decremente"})}>decremente</button>
    <button onClick ={() => dispatch({type : "reset"})}>reset</button>
    </div>
  );
}
```