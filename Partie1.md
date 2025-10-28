# Cours AJAX - Jour 1 : Première Conversation avec une API

## Accueil et Présentation

### Faire connaissance

- Quel est votre niveau en JavaScript ? Du HTML/CSS ?
- Avez-vous déjà entendu parler d'AJAX ?

### Objectifs de la formation

Voilà une vue d'ensemble des notions que l'on va apprendre ensemble.

1. **Comprendre AJAX et les APIs** : Comment les sites modernes fonctionnent
2. **Faire votre première requête** : Communiquer avec un serveur sans recharger la page
3. **Manipuler des données** : Récupérer et afficher des informations dynamiques
4. **Créer des applications interactives** : Projets concrets et visuels

**Instant Notions**

- API : Application Programming Interface (interface de programmation)
- JSON : JavaScript Object Notation (format de données)
- Asynchrone : Le code qui ne bloque pas l'exécution

### Le pitch du cours

**En 3 jours, vous allez coder votre propre mini-Netflix !**

- **Jour 1** : Apprendre à communiquer avec des APIs
- **Jour 2** : Créer un moteur de recherche de films
- **Jour 3** : Ajouter des trailers YouTube et finaliser

### Méthode pédagogique

- **30% théorie, 70% pratique** : on code ensemble !
- **Participation active** : "Coupez"-moi la parole, pas de monologue d'enseignant
- **Projets concrets** : Applications fun et visuelles
- **Découpages des chapitres** : "L'essentiel" (pour tous) et "ALLONS PLUS LOIN" (bonus)

### Quelles bonnes pratiques appliquez-vous ?

En voilà quelques unes essentielles :

- Il n'y a pas de mauvaise question (PAS DU TOUT)
- Les moteurs de recherche sont nos amis
- L'anglais 🇬🇧
- L'IA, quelle est votre usage ?
- La pratique est cruciale !
- Collaborer donne des ailes

---

## Planning de la Journée

- Introduction : Le Problème qu'AJAX Résout
- HTTP & APIs : Comment Ça Marche ?
- Première Requête avec Fetch
- JSON & Manipulation du DOM
- Projet Fil Rouge : Quote & Cat Gallery

---

## 1 - Le Problème qu'AJAX Résout

### L'essentiel

#### Le Web Avant AJAX

Imaginez : vous remplissez un formulaire d'inscription en ligne avec 10 champs. Vous cliquez sur "Envoyer"...

**Et toute la page se recharge. 💥**

Si vous avez oublié un champ ? Tout se recharge. Vous voulez voir plus de contenu ? Toute la page se recharge.

👉 **Problème** : Navigation lente, expérience utilisateur frustrante, gaspillage de bande passante.

#### Le Web avec AJAX

Maintenant, pensez à Instagram : vous scrollez, de nouvelles photos apparaissent. Pas de rechargement de page.

Gmail : vous envoyez un email, il part en arrière-plan. Vous continuez à utiliser l'interface.

Google Maps : vous déplacez la carte, de nouvelles zones se chargent dynamiquement.

👉 **Solution** : AJAX permet de communiquer avec un serveur **SANS recharger la page entière**.

#### Démo

Je vais vous montrer la différence :

**Site SANS AJAX :**

- Clic sur un bouton → Toute la page se recharge
- Temps d'attente visible
- Perte du contexte (scroll position, etc.)

**Site AVEC AJAX :**
- Clic sur un bouton → Seule une partie se met à jour
- Instantané et fluide
- L'utilisateur reste dans son flow

**Question à la classe :** "Quels autres exemples de sites avec AJAX connaissez-vous ?"

#### Qu'est-ce qu'AJAX ?

**AJAX = Asynchronous JavaScript And XML**

Décomposons :
- **Asynchronous** : Le code qui ne bloque pas (on verra ça)
- **JavaScript** : Le langage qu'on utilise
- **And XML** : Format de données... **dépassé !**

⚠️ **Note historique** : Le "XML" dans AJAX est historique. Aujourd'hui on utilise principalement **JSON** (plus simple, plus léger).

**En bref :**

AJAX, c'est utiliser JavaScript pour :
1. Envoyer une requête à un serveur
2. Recevoir des données
3. Mettre à jour la page
4. **Le tout sans rechargement !**

### ALLONS PLUS LOIN

#### Histoire d'AJAX

- **1999** : Microsoft invente `XMLHttpRequest` pour Outlook Web Access
- **2005** : Le terme "AJAX" est popularisé par Jesse James Garrett
- **2015** : `fetch()` arrive (modern, plus simple)
- **Aujourd'hui** : AJAX est partout, indispensable pour le web moderne

#### XMLHttpRequest vs Fetch

Dans ce cours, on utilisera **Fetch** (moderne, plus clean).

`XMLHttpRequest` existe toujours mais est considéré comme l'ancienne méthode. Si vous voyez du vieux code sur StackOverflow avec `XMLHttpRequest`, sachez que c'est l'ancêtre de `fetch()`.

**Instant Veille outils :**

Connaissez-vous ces alternatives ?
- [Axios](https://axios-http.com/) : Librairie populaire (alternative à Fetch)
- [React Query](https://tanstack.com/query) : Pour gérer les données dans React
- [SWR](https://swr.vercel.app/) : Autre librairie de data fetching

---

## 2 - HTTP & APIs : Comment Ça Marche ?

### L'essentiel

#### Le Modèle Client-Serveur

**Analogie du restaurant :**

Imaginez que vous êtes dans un restaurant :
- **Vous (Client)** : "Je voudrais le menu du jour"
- **Serveur** : Va en cuisine, revient avec le menu
- **Vous** : Consultez le menu
- **Vous** : "Je voudrais une pizza margherita"
- **Serveur** : Va en cuisine, revient avec la pizza

**Sur le web, c'est pareil :**

- **Votre navigateur (Client)** : Demande des données
- **Serveur** : Traite la demande, envoie la réponse
- **Votre navigateur** : Affiche les données

```
Client (Navigateur) → Requête → Serveur
                    ← Réponse ←
```

#### Qu'est-ce qu'une API ?

**API = Application Programming Interface**

C'est un "menu" que le serveur met à disposition. Au lieu de dire "je veux une pizza", on dit "je veux les données de l'utilisateur 123".

**Exemple concret :**

- **URL** : `https://api.quotable.io/quotes/random`
- **Ce que ça fait** : Donne une citation aléatoire
- **Gratuit** : Pas d'inscription, pas de clé

👉 Une API, c'est comme un restaurant qui publie son menu en ligne. N'importe qui peut le consulter et commander.

#### Les Méthodes HTTP

Quand vous commandez au restaurant, vous pouvez :
- **Consulter** le menu : GET
- **Commander** un plat : POST
- **Modifier** votre commande : PUT
- **Annuler** votre commande : DELETE

**Les 4 verbes principaux :**

| Méthode | Action | Analogie Restaurant |
|---------|--------|---------------------|
| **GET** | Récupérer des données | "Je veux voir le menu" |
| **POST** | Envoyer/créer des données | "Je commande une pizza" |
| **PUT** | Modifier des données | "Changez ma pizza en calzone" |
| **DELETE** | Supprimer des données | "Annulez ma commande" |

**Aujourd'hui, on va surtout utiliser GET** (récupérer des données).

#### Les Codes de Statut HTTP

Le serveur répond toujours avec un **code de statut** :

| Code | Signification | Analogie Restaurant |
|------|---------------|---------------------|
| **200** | OK | ✅ "Votre plat arrive" |
| **404** | Not Found | ❌ "On n'a pas ce plat au menu" |
| **500** | Server Error | 💥 "La cuisine a brûlé" |
| **401** | Unauthorized | 🔒 "Vous devez être membre" |

**Les codes commencent par :**
- **2xx** : Succès 🎉
- **4xx** : Erreur de votre côté (client)
- **5xx** : Erreur côté serveur

### ALLONS PLUS LOIN

#### REST API

La plupart des APIs modernes suivent le principe **REST** (Representational State Transfer).

**Principes REST :**

- Utilise les méthodes HTTP standards (GET, POST, etc.)
- URLs claires et logiques : `/users/123`, `/movies/550`
- Sans état (stateless) : chaque requête est indépendante

#### Headers HTTP

Les requêtes et réponses contiennent des **headers** (en-têtes), comme les métadonnées.

**Exemples courants :**

```javascript
Content-Type: application/json  // Type de données
Authorization: Bearer TOKEN      // Authentification
Accept: application/json         // Ce qu'on accepte
```

On verra ça en pratique plus tard.

#### HTTPS vs HTTP

- **HTTP** : Non sécurisé (🔓)
- **HTTPS** : Sécurisé avec chiffrement (🔒)

Aujourd'hui, presque tout le web utilise HTTPS. Toutes nos APIs du cours aussi.

---

## 3 - Première Requête avec Fetch

### Instant Terminal

> Ouvrir le terminal "git bash"

- `mkdir mon_dossier`: make directory -> permet de créer un dossier `mon_dossier`
- `cd Documents/mes_scripts/` : change directory -> se déplacer vers un dossier `Documents/mes_scripts/`  . Pour aller dans un dossier parent, faire `cd ..` (ou `../../../` etc)
- `pwd` : voir le chemin où je me situe
- `ls` : lister ce qui est dans mon dossier / OU ls -a
- `touch monFichier`: créer un fichier `monFichier` 
- `mv` : move / déplacer un élément (fichier ou dossier) / OU renommer un dossier
- `rm monFichier` : supprime un fichier / OU `rm -r monDossier` supprimer mon dossier
- `cp` : copy -> copier un fichier vers un autre endroit, ex: `cp abc Documents/abc` copie abc vers `Documents/abc`

Raccourcis Terminal

- ctrl u : supprimer toute la ligne
- ctrl a : aller au début de la ligne
- ctrl e : aller à la fin de la ligne
- ctrl w : pour supprimer un mot de droite à gauche

### L'essentiel

#### Qu'est-ce que Fetch ?

`fetch()` est une fonction JavaScript moderne pour faire des requêtes HTTP.

En savoir plus sur fetch => [ici](https://www.w3schools.com/js/js_api_fetch.asp)

**Syntaxe de base :**

```javascript
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
```

Décortiquons ligne par ligne.

#### Notre Première API : Quotable

On va utiliser l'API **Quotable** qui donne des citations aléatoires.

**URL :** `https://api.quotable.io/quotes/random`

**Testons dans la console du navigateur :**

1. Ouvrir Chrome/Firefox
2. Aller sur n'importe quelle page
3. Ouvrir la console (F12 → Console)
4. Taper :

```javascript
fetch('https://api.quotable.io/quotes/random')
```

ou

```javascript
fetch('https://api.thecatapi.com/v1/images/search')
```

**Résultat :** `Promise {<pending>}`

**Mais j'ai pas compris, c'est quoi cette Promise ?**

#### Les Promesses : Comprendre l'Asynchrone

Imagine que tu commandes une pizza par téléphone :
1. Tu appelles la pizzeria : **fetch()**
2. Ils te disent "Ok, on prépare" : **Promise {<pending>}**
3. Tu **ne restes pas au téléphone à attendre**, tu fais autre chose
4. Quand la pizza arrive : **Promise {<fulfilled>}**
5. Tu récupères ta pizza : **.then()**

**En JavaScript :**

```javascript
fetch('https://api.quotable.io/quotes/random')  // J'appelle la pizzeria
  .then(response => {                            // Quand la réponse arrive
    console.log(response);                       // Je la reçois
  })
```

**Important :** Le code JavaScript ne s'arrête pas pendant le fetch. Il continue, et quand la réponse arrive, le `.then()` s'exécute.

C'est ça, **l'asynchrone** : ne pas bloquer l'exécution.

#### Récupérer les Données JSON

Essayons à nouveau dans la console :

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => {
    console.log(response);
  })
```

**Résultat :** Un objet `Response` avec plein d'infos.

**Mais où est ma citation ?**

Il faut **parser** la réponse en JSON :

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())  // Parser le JSON
  .then(data => {
    console.log(data);                // Maintenant on a les données !
  })
```

**Résultat :**
```javascript
{
  _id: "abc123",
  content: "The only way to do great work is to love what you do.",
  author: "Steve Jobs",
  tags: ["famous-quotes"],
  length: 51
}
```

🎉 **Vous venez de faire votre première requête AJAX !**

#### Décortiquons le Code

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())
  .then(data => console.log(data))
```

**Ligne 1 :** `fetch(url)`
- Envoie une requête GET à l'URL
- Retourne une **Promise**

**Ligne 2 :** `.then(response => response.json())`
- Quand la réponse arrive
- `response.json()` convertit la réponse en objet JavaScript
- Retourne une **nouvelle Promise**

**Ligne 3 :** `.then(data => console.log(data))`
- Quand le parsing JSON est fini
- `data` contient notre objet JavaScript
- On peut l'utiliser !

#### Exercice Pratique

À vous de jouer ! Dans la console :

1. Faire un `fetch()` vers `https://api.quotable.io/quotes/random`
2. Parser le JSON
3. Afficher juste l'auteur de la citation (`data.author`)

**Solution :**
```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())
  .then(data => {
    console.log(data.author);
  })
```

💡 **Astuce :** Rafraîchissez plusieurs fois (ou relancez la commande), vous aurez des citations différentes !

### ALLONS PLUS LOIN (mais ça reste vital de le savoir)

#### Gestion d'Erreurs avec .catch()

Que se passe-t-il si l'API ne répond pas ? Si l'URL est fausse ?

**Bonne pratique :** Toujours ajouter `.catch()` pour gérer les erreurs.

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Erreur:', error);
  })
```

**Testez avec une URL invalide :**

```javascript
fetch('https://api.fausse-url-qui-existe-pas.com')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => {
    console.error('Oups !', error);  // L'erreur est attrapée ici
  })
```

#### Vérifier le Statut de la Réponse

Même si la requête "fonctionne", le serveur peut retourner une erreur (404, 500, etc.).

**Bonne pratique :** Vérifier `response.ok`

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => {
    if (!response.ok) {
      throw new Error(`Erreur HTTP: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Erreur:', error))
```

---

## 4 - JSON & Manipulation du DOM

### L'essentiel

#### Qu'est-ce que JSON ?

**JSON = JavaScript Object Notation**

C'est un format de données texte, facile à lire pour les humains ET les machines.

**Exemple de JSON :**

```json
{
  "name": "Alice",
  "age": 25,
  "hobbies": ["coding", "reading", "gaming"]
}
```

**Ressemble à un objet JavaScript, non ?** C'est volontaire !

**Différences :**

- JSON : Clés entre **guillemets** `"name"`
- JavaScript : Clés sans guillemets `name` (optionnel)

**Conversions :**

```javascript
// Objet JS → JSON (string)
const user = { name: "Alice", age: 25 };
const jsonString = JSON.stringify(user);
console.log(jsonString);  // '{"name":"Alice","age":25}'

// JSON (string) → Objet JS
const jsonString = '{"name":"Alice","age":25}';
const user = JSON.parse(jsonString);
console.log(user.name);  // "Alice"
```

👉 **Avec Fetch, `response.json()` fait le parsing automatiquement !**

#### Explorer la Structure JSON

Regardons la réponse de notre API Quotable :

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())
  .then(data => {
    console.log('Citation:', data.content);
    console.log('Auteur:', data.author);
    console.log('Longueur:', data.length);
    console.log('Tags:', data.tags);
  })
```

**Résultat :**

```
Citation: The only way to do great work is to love what you do.
Auteur: Steve Jobs
Longueur: 51
Tags: ["famous-quotes"]
```

**Pour accéder aux propriétés :**
- `data.content` → La citation
- `data.author` → L'auteur
- `data.tags` → Array de tags
- `data.tags[0]` → Premier tag

Typiquement cette technique nous permet de récupérer une info précise dans un objet qui pourrait être immense. Ex: 

```js
const data = {
  user: {
    pictures: {
      cat_pictures: [
        { name: "pic1", url: "https://cat.com" },
        { name: "pic2", url: "https://cat.com" },
        { name: "pic3", url: "https://cat.com" },
        { name: "pic4", url: "https://cat.com" },
      ],
    },
  },
};
```

Pour récupérer la dernière url de chat dispo, on ferait:

```javascript
console.log(data.user.pictures.cat_pictures[3].url);
```



#### Afficher dans la Page HTML

Maintenant, sortons de la console et affichons dans une vraie page web !

**Créons notre premier fichier HTML :**

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Quote Generator</title>
</head>
<body>
  <h1>Citation du Jour</h1>

  <div id="quote-display">
    <!-- La citation s'affichera ici -->
  </div>

  <button id="new-quote-btn">Nouvelle Citation</button>

  <script src="script.js"></script>
</body>
</html>
```

**Dans script.js :**

```javascript
fetch('https://api.quotable.io/quotes/random')
  .then(response => response.json())
  .then(data => {
    // Sélectionner l'élément où afficher
    const quoteDisplay = document.querySelector('#quote-display');

    // Mettre à jour le contenu
    quoteDisplay.innerHTML = `
      <p>"${data.content}"</p>
      <p>— ${data.author}</p>
    `;
  })
  .catch(error => console.error('Erreur:', error));
```

**Ouvrez index.html dans votre navigateur.** Vous voyez une citation !

#### Template Literals

Vous avez remarqué les **backticks** `` ` `` et `${}` ?

Ce sont des **template literals** (littéraux de gabarit), très pratiques pour insérer des variables dans des strings.

**Sans template literals (ancien) :**
```javascript
const html = '<p>"' + data.content + '"</p><p>— ' + data.author + '</p>';
```

**Avec template literals (moderne) :**

```javascript
const html = `
  <p>"${data.content}"</p>
  <p>— ${data.author}</p>
`;
```

Beaucoup plus lisible ! 🎉

🔗 [Aide : Template literals sur MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Template_literals)

#### Ajouter un Event Listener

Maintenant, faisons en sorte que le bouton génère une nouvelle citation :

```javascript
// Fonction pour récupérer et afficher une citation
function fetchQuote() {
  fetch('https://api.quotable.io/quotes/random')
    .then(response => response.json())
    .then(data => {
      const quoteDisplay = document.querySelector('#quote-display');
      quoteDisplay.innerHTML = `
        <p>"${data.content}"</p>
        <p>— ${data.author}</p>
      `;
    })
    .catch(error => console.error('Erreur:', error));
}

// Appeler au chargement de la page
fetchQuote();

// Ajouter un event listener sur le bouton
const btn = document.querySelector('#new-quote-btn');
btn.addEventListener('click', fetchQuote);
```

**Testez !** Chaque clic donne une nouvelle citation.

### ALLONS PLUS LOIN

#### Créer des Éléments avec createElement

Au lieu d'utiliser `innerHTML` (qui remplace tout), on peut créer des éléments précis :

```javascript
function displayQuote(data) {
  const quoteDisplay = document.querySelector('#quote-display');

  // Vider le contenu précédent
  quoteDisplay.innerHTML = '';

  // Créer les éléments
  const quotePara = document.createElement('p');
  quotePara.textContent = `"${data.content}"`;
  quotePara.className = 'quote-text';

  const authorPara = document.createElement('p');
  authorPara.textContent = `— ${data.author}`;
  authorPara.className = 'quote-author';

  // Ajouter au DOM
  quoteDisplay.appendChild(quotePara);
  quoteDisplay.appendChild(authorPara);
}
```

**Avantages :**
- Plus sûr (pas de risque d'injection HTML)
- Permet d'ajouter des classes, événements, etc.

#### Loading State

Pendant que le fetch s'exécute, l'utilisateur ne voit rien. Ajoutons un indicateur de chargement :

```javascript
function fetchQuote() {
  const quoteDisplay = document.querySelector('#quote-display');

  // Afficher "Chargement..."
  quoteDisplay.innerHTML = '<p>🔄 Chargement...</p>';

  fetch('https://api.quotable.io/quotes/random')
    .then(response => response.json())
    .then(data => {
      quoteDisplay.innerHTML = `
        <p>"${data.content}"</p>
        <p>— ${data.author}</p>
      `;
    })
    .catch(error => {
      quoteDisplay.innerHTML = '<p>❌ Erreur de chargement</p>';
      console.error('Erreur:', error);
    });
}
```

Maintenant l'UX est meilleure !

---

## 5 - Projet Fil Rouge : Quote & Cat Gallery

### L'essentiel

#### Énoncé du Projet

**Objectif :** Créer une application fun qui affiche ~~des citations aléatoires ET~~ une galerie de photos de chats.

![CatQuatre_P1](/home/josselin/Dev/tmp_notes/educative_ajax/assets/CatQuatre_P1.png)

![CatQuatre_P2](/home/josselin/Dev/tmp_notes/educative_ajax/assets/CatQuatre_P2.png)



**Pourquoi ce projet ?**

- Pratique de 2 APIs différentes
- Révision HTML/CSS
- Manipulation du DOM
- Gratification visuelle immédiate !

**Fonctionnalités minimales :**

1. **Section "Citation du Jour"**
   - Afficher une citation aléatoire
   - Bouton "Nouvelle Citation"

2. **Section "Galerie de Chats"**
   - Afficher 3 photos de chats aléatoires
   - Bouton "Nouveaux Chats"

3. **Design sympa**
   - Deux sections distinctes
   - Cards pour la citation
   - Grille pour les chats
   - Couleurs, espacements

**Extensions possibles :**

**Niveau 1 :**
- Compteur de citations générées
- Animation au changement de citation
- Responsive design

**Niveau 2 :**
- Filtrer les citations par tag
- Favoris (localStorage - on verra ça Jour 2)
- Charger plus de chats (5, 10...)

**Niveau 3 :**
- Plusieurs pages (Home, Citations, Chats)
- Search bar pour chercher par auteur
- Partager une citation (copier dans le clipboard)

#### APIs à Utiliser

**1. Quotable API (Citations)**
- **URL :** `https://api.quotable.io/quotes/random`
- **Pas de clé nécessaire**
- **Retourne :** Un objet avec `content`, `author`, `tags`, etc.

**2. The Cat API (Photos de Chats)**
- **URL :** `https://api.thecatapi.com/v1/images/search?limit=3`
- **Pas de clé nécessaire**
- **Retourne :** Un array de 3 objets avec `url`, `width`, `height`

🔗 [Documentation Quotable](https://github.com/lukePeavey/quotable)
🔗 [Documentation The Cat API](https://docs.thecatapi.com)

#### Structure du Projet

Vous allez créer **de A à Z** (pas de starter fourni) :

```bash
quote-cat-app/
├── index.html
├── style.css
├── script.js
└── README.md
```

#### Setup du Projet

**Étape 1 : Créer le Dossier et les Fichiers**

1. Créer un dossier `quote-cat-app` sur votre bureau
2. Ouvrir ce dossier dans VS Code
3. Créer les fichiers :
   - `index.html`
   - `style.css`
   - `script.js`

💡 **Astuce :** Vous pouvez créer des fichiers directement dans VS Code (Fichier → Nouveau ou clic droit dans l'explorateur)

**Étape 2 : Structure HTML de Base**

Dans `index.html`, créer la structure :
- DOCTYPE, html, head, body
- Lier le CSS (`<link rel="stylesheet" href="style.css">`)
- Lier le JS (`<script src="script.js"></script>` avant `</body>`)
- Titre de la page

💡 **Astuce :** Tapez `!` puis Tab dans VS Code pour générer le squelette HTML

**Étape 3 : Créer les Sections**

Dans le `<body>`, créer :
- Un `<header>` avec le titre de l'app
- Une `<section id="quotes">` pour les citations
  - Un `<div id="quote-display"></div>` où la citation s'affichera
  - Un `<button id="new-quote-btn">Nouvelle Citation</button>`
- Une `<section id="cats">` pour les chats
  - Un `<div id="cats-gallery"></div>` où les photos s'afficheront
  - Un `<button id="new-cats-btn">Nouveaux Chats</button>`

**Étape 4 : CSS de Base**

Dans `style.css`, ajouter :
- Reset de marges/paddings (`* { margin: 0; padding: 0; box-sizing: border-box; }`)
- Couleur de fond pour le body
- Centrer le contenu
- Espacements entre sections

**Étape 5 : Tester la Structure**

Ouvrir `index.html` dans le navigateur. Vous devez voir votre structure vide, c'est normal !

Dans `script.js`, ajouter :
```javascript
console.log('Script chargé !');
```

Ouvrir la console (F12). Si vous voyez le message, tout est bien lié !

#### Instructions Guidées - Partie Citations

**Étape 1 : Fonction fetchQuote()**

Créer une fonction `fetchQuote()` qui :
1. Fait un `fetch()` vers `https://api.quotable.io/quotes/random`
2. Parse la réponse en JSON
3. Affiche dans la console : `content` et `author`

💡 **Indice :** Relisez la partie "Première Requête avec Fetch" de ce cours

🔗 [Aide : Utiliser Fetch sur MDN](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch)

**Étape 2 : Afficher dans le DOM**

Modifier `fetchQuote()` pour :
1. Sélectionner `#quote-display`
2. Utiliser `innerHTML` avec un template literal
3. Afficher la citation entre guillemets et l'auteur en dessous

**Code exemple :**
```javascript
quoteDisplay.innerHTML = `
  <p class="quote-text">"${data.content}"</p>
  <p class="quote-author">— ${data.author}</p>
`;
```

💡 **Indice :** Ajoutez des classes pour pouvoir styliser après

**Étape 3 : Bouton "Nouvelle Citation"**

1. Sélectionner `#new-quote-btn`
2. Ajouter un event listener `click`
3. Appeler `fetchQuote()` au clic

🔗 [Aide : addEventListener sur MDN](https://developer.mozilla.org/fr/docs/Web/API/EventTarget/addEventListener)

**Étape 4 : Appel Initial**

Appeler `fetchQuote()` une fois au chargement de la page pour avoir une citation d'entrée.

**Testez !** Vous devez voir une citation au chargement, et pouvoir en générer de nouvelles avec le bouton.

#### Instructions Guidées - Partie Chats

**Étape 1 : Fonction fetchCats()**

Créer une fonction `fetchCats()` qui :
1. Fait un `fetch()` vers `https://api.thecatapi.com/v1/images/search?limit=3`
2. Parse la réponse en JSON
3. Affiche le array dans la console

⚠️ **Attention :** L'API retourne un **array** de 3 objets, pas un seul objet !

**Étape 2 : Boucler sur les Résultats**

L'API retourne :
```javascript
[
  { url: "https://...", width: 1200, height: 800 },
  { url: "https://...", width: 1000, height: 600 },
  { url: "https://...", width: 800, height: 600 }
]
```

Utiliser `.forEach()` pour parcourir le array :

```javascript
.then(cats => {
  cats.forEach(cat => {
    console.log(cat.url);  // URL de l'image
  });
})
```

🔗 [Aide : Array.forEach() sur MDN](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

**Étape 3 : Créer les Images**

Pour chaque chat, créer un élément `<img>` :

```javascript
.then(cats => {
  const gallery = document.querySelector('#cats-gallery');
  gallery.innerHTML = '';  // Vider la galerie

  cats.forEach(cat => {
    const img = document.createElement('img');
    img.src = cat.url;
    img.alt = 'Photo de chat';
    gallery.appendChild(img);
  });
})
```

🔗 [Aide : createElement sur MDN](https://developer.mozilla.org/fr/docs/Web/API/Document/createElement)

**Étape 4 : Bouton "Nouveaux Chats"**

Même logique que pour les citations :
1. Sélectionner `#new-cats-btn`
2. Event listener `click`
3. Appeler `fetchCats()`

**Étape 5 : Appel Initial**

Appeler `fetchCats()` au chargement pour avoir des chats d'entrée.

**Testez !** Vous devez voir 3 chats au chargement, et pouvoir en charger de nouveaux avec le bouton.

#### Stylisation CSS

Maintenant que tout fonctionne, rendons ça beau !

**Idées de styles :**

**Pour la citation :**
```css
#quotes {
  background: #f0f0f0;
  padding: 40px;
  border-radius: 10px;
  margin: 20px;
}

.quote-text {
  font-size: 1.5em;
  font-style: italic;
  margin-bottom: 10px;
}

.quote-author {
  text-align: right;
  color: #666;
}
```

**Pour la galerie de chats (CSS Grid) :**
```css
#cats-gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin: 20px 0;
}

#cats-gallery img {
  width: 100%;
  height: 300px;
  object-fit: cover;
  border-radius: 10px;
}
```

🔗 [Aide : CSS Grid sur CSS-Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/)

**Pour les boutons :**
```css
button {
  background: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
}

button:hover {
  background: #0056b3;
}
```

À vous de personnaliser avec vos couleurs préférées !

#### Gestion d'Erreurs

Ajoutez des `.catch()` à vos fetch pour gérer les erreurs :

```javascript
.catch(error => {
  console.error('Erreur:', error);
  alert('Oups, quelque chose s\'est mal passé !');
});
```

**Testez avec une URL invalide** pour voir si vos erreurs sont bien gérées.

#### README

Créer un `README.md` avec :
- Titre du projet
- Description : "Application qui affiche des citations et photos de chats aléatoires"
- Technologies utilisées : HTML, CSS, JavaScript, Fetch API
- APIs utilisées : Quotable, The Cat API
- Comment lancer : "Ouvrir index.html dans un navigateur"
- Screenshot (optionnel mais sympa)

### ALLONS PLUS LOIN

#### Compteur de Citations

Ajouter un compteur qui s'incrémente à chaque nouvelle citation :

```javascript
let quoteCount = 0;

function fetchQuote() {
  // ... fetch ...
  .then(data => {
    quoteCount++;
    quoteDisplay.innerHTML = `
      <p class="quote-text">"${data.content}"</p>
      <p class="quote-author">— ${data.author}</p>
      <p class="quote-count">Citation #${quoteCount}</p>
    `;
  })
}
```

#### Animation au Changement

Ajouter une classe CSS avec animation :

```css
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.quote-text {
  animation: fadeIn 0.5s ease;
}
```

#### Filtrer par Tag

L'API Quotable permet de filtrer :

```javascript
// Citation sur la technologie
fetch('https://api.quotable.io/quotes/random?tags=technology')
```

Ajouter des boutons "Technologie", "Inspiration", "Humour", etc.

#### Responsive Design

Adapter la grille de chats pour mobile :

```css
@media (max-width: 768px) {
  #cats-gallery {
    grid-template-columns: 1fr;  /* 1 colonne sur mobile */
  }
}
```

---

## Récapitulatif Jour 1

### Ce qu'on a appris aujourd'hui

1. **Le problème qu'AJAX résout** : Communication serveur sans rechargement de page
2. **HTTP & APIs** : Client-Serveur, méthodes GET/POST/PUT/DELETE, codes de statut
3. **Fetch API** : Faire des requêtes HTTP en JavaScript
4. **Promesses & Asynchrone** : `.then()`, `.catch()`, comprendre l'asynchrone
5. **JSON** : Format de données, parsing avec `response.json()`
6. **Manipulation du DOM** : Afficher des données dynamiques, event listeners
7. **Projet concret** : Application Quote & Cat Gallery

### Points d'attention pour demain

- **Finir le projet Fil Rouge** si vous n'avez pas terminé
- **Expérimenter** : Essayez d'autres APIs publiques !
- **Réviser** les promesses et `.then()` - on va les remplacer par `async/await` demain
- Demain on attaque **Netflix** ! 🎬

### Instant Cheat Sheet! => Fetch Recap

```javascript
// Requête GET simple
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error))

// Avec gestion d'erreurs HTTP
fetch(url)
  .then(response => {
    if (!response.ok) throw new Error('Erreur HTTP');
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error(error))

// Afficher dans le DOM
fetch(url)
  .then(response => response.json())
  .then(data => {
    document.querySelector('#element').innerHTML = `
      <p>${data.property}</p>
    `;
  })
```

### Ressources pour Aller Plus Loin

**Documentation & Tutoriels :**
- [MDN : Utiliser Fetch](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API/Using_Fetch)
- [JavaScript.info : Fetch](https://javascript.info/fetch)
- [Liste d'APIs Publiques](https://github.com/public-apis/public-apis)
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) : API de test gratuite

---

#### Projet Bonus : Recipe Finder 🍕🍰

**Si vous voulez aller plus loin**, voici un projet supplémentaire pour pratiquer avec une API différente !

**API : TheMealDB**
- **URL :** `https://www.themealdb.com/api/json/v1/1/random.php`
- **Gratuite**, pas de clé nécessaire
- **Retourne :** Une recette aléatoire complète

**Fonctionnalités à implémenter :**

1. **Bouton "Recette Aléatoire"**
   - Afficher une recette au hasard
   - Photo du plat, nom, catégorie

2. **Afficher les Détails**
   - Liste d'ingrédients
   - Instructions de préparation
   - Zone géographique (origine)

3. **Search Bar (avancé)**
   - Endpoint : `https://www.themealdb.com/api/json/v1/1/search.php?s=chicken`
   - Chercher par nom de plat

**Structure de réponse :**

```javascript
{
  "meals": [
    {
      "idMeal": "52772",
      "strMeal": "Teriyaki Chicken Casserole",
      "strCategory": "Chicken",
      "strArea": "Japanese",
      "strInstructions": "Preheat oven to 350°F...",
      "strMealThumb": "https://www.themealdb.com/images/media/meals/wvpsxx1468256321.jpg",
      "strIngredient1": "soy sauce",
      "strMeasure1": "3/4 cup"
      // ... jusqu'à strIngredient20
    }
  ]
}
```

**Conseil :** Les ingrédients vont de `strIngredient1` à `strIngredient20`. Utiliser une boucle pour les récupérer !

**Exemple de boucle pour les ingrédients :**

```javascript
const ingredients = [];
for (let i = 1; i <= 20; i++) {
  const ingredient = meal[`strIngredient${i}`];
  const measure = meal[`strMeasure${i}`];

  if (ingredient && ingredient.trim() !== '') {
    ingredients.push(`${measure} ${ingredient}`);
  }
}
```

**Extensions possibles :**
- Filtrer par catégorie (Dessert, Seafood, Vegetarian...)
- Filtrer par pays (Italian, Chinese, Mexican...)
- Liste de favoris avec localStorage

🔗 [Documentation TheMealDB](https://www.themealdb.com/api.php)

**Niveau de difficulté :** Similaire au projet Quote & Cat Gallery, mais avec une structure de données plus complexe (bon entraînement pour demain !).

---

**Bravo pour cette première journée ! 🎉**

**Rendez-vous demain pour créer votre Netflix !** 🍿