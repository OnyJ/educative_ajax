# Cours AJAX - Jour 2 : Netflix Phase 1 - Search & Display

## Planning de la Journée

- Démos Jour 1
- Async/Await : Syntaxe Plus Claire
- Formulaires & preventDefault
- LocalStorage : Sauvegarder des Données
- Introduction Projet Netflix
- Projet Netflix Phase 1 : Search + Grille de Films

---

## 1 - Démos Jour 1

### L'essentiel

#### Démos Rapides

**Questions rapides :**
- Quels ont été vos blocages principaux hier ?
- Qu'est-ce qui était le plus difficile : Fetch, JSON, ou DOM ?

#### Déblocage Collectif

Passons 15-20 minutes à débloquer les problèmes courants.

**Erreurs fréquentes vues hier :**

**1. "Mes images de chats ne s'affichent pas"**

Vérifiez :
- Utilisez-vous bien `.forEach()` pour parcourir l'array ?
- Avez-vous bien `img.src = cat.url` (pas `cat.image`) ?
- Les images s'ajoutent-elles au DOM avec `appendChild()` ?

**2. "Ma citation ne se met pas à jour"**

Vérifiez :
- Le bouton a-t-il un event listener ?
- La fonction `fetchQuote()` est-elle bien appelée au clic ?
- Utilisez-vous `innerHTML` pour remplacer le contenu ?

**3. "J'ai une erreur CORS"**

Si vous voyez `blocked by CORS policy` :
- Vérifiez l'URL de l'API (typo ?)
- Les APIs du cours (Quotable, Cat API) supportent CORS
- Si vous testez une autre API, elle n'autorise peut-être pas les requêtes depuis le navigateur

#### Instant Cheat Sheet! => Révisions Fetch

```javascript
// Pattern complet avec gestion d'erreurs
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Erreur:', error);
    return null;
  }
}
```

---

## 2 - Async/Await : Syntaxe Plus Claire

### L'essentiel

#### Le Problème avec .then()

Hier, on a utilisé `.then()` pour gérer les promesses :

```javascript
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error))
```

C'est correct, mais ça devient vite complexe :

```javascript
// Plusieurs requêtes successives = callback hell
fetch(url1)
  .then(response => response.json())
  .then(data1 => {
    fetch(url2)
      .then(response => response.json())
      .then(data2 => {
        fetch(url3)
          .then(response => response.json())
          .then(data3 => {
            // 😵 Difficile à lire !
          })
      })
  })
```

#### La Solution : async/await

`async/await` rend le code **asynchrone ressembler à du code synchrone**.

**Avant (.then()) :**

```javascript
function fetchQuote() {
  fetch('https://api.quotable.io/quotes/random')
    .then(response => response.json())
    .then(data => {
      console.log(data.content);
    })
    .catch(error => {
      console.error('Erreur:', error);
    });
}
```

**Après (async/await) :**

```javascript
async function fetchQuote() {
  try {
    const response = await fetch('https://api.quotable.io/quotes/random');
    const data = await response.json();
    console.log(data.content);
  } catch (error) {
    console.error('Erreur:', error);
  }
}
```

**Beaucoup plus lisible !** 🎉

#### Comment ça Marche ?

**Deux mots-clés :**

1. **`async`** : Déclare une fonction asynchrone
   ```javascript
   async function maFonction() {
     // Cette fonction retourne une Promise
   }
   ```

2. **`await`** : Attend qu'une Promise se resolve
   ```javascript
   const data = await fetch(url);  // Attend la réponse
   ```

**Important :** `await` ne peut être utilisé **que dans une fonction `async`**.

#### Réécrire Nos Exemples d'Hier

**Quote Generator avec async/await :**

```javascript
async function fetchQuote() {
  try {
    const response = await fetch('https://api.quotable.io/quotes/random');

    if (!response.ok) {
      throw new Error(`Erreur HTTP: ${response.status}`);
    }

    const data = await response.json();

    const quoteDisplay = document.querySelector('#quote-display');
    quoteDisplay.innerHTML = `
      <p>"${data.content}"</p>
      <p>— ${data.author}</p>
    `;
  } catch (error) {
    console.error('Erreur:', error);
    alert('Impossible de charger une citation');
  }
}
```

**Cat Gallery avec async/await :**

```javascript
async function fetchCats() {
  try {
    const response = await fetch('https://api.thecatapi.com/v1/images/search?limit=3');
    const cats = await response.json();

    const gallery = document.querySelector('#cats-gallery');
    gallery.innerHTML = '';

    cats.forEach(cat => {
      const img = document.createElement('img');
      img.src = cat.url;
      gallery.appendChild(img);
    });
  } catch (error) {
    console.error('Erreur:', error);
  }
}
```

#### Try/Catch pour Gérer les Erreurs

Avec async/await, on utilise `try/catch` (au lieu de `.catch()`) :

```javascript
try {
  // Code qui peut échouer
  const response = await fetch(url);
  const data = await response.json();
} catch (error) {
  // Si erreur, on arrive ici
  console.error('Oups:', error);
}
```

**Analogie :** C'est comme dire "Essaye de faire ça, et si ça rate, fais plutôt ça".

#### Exercice Pratique

Reprenez votre projet d'hier et convertissez toutes vos fonctions avec `.then()` en `async/await`.

**Avant :**
```javascript
function loadData() {
  fetch(url)
    .then(res => res.json())
    .then(data => displayData(data))
    .catch(err => console.error(err));
}
```

**Après :**
```javascript
async function loadData() {
  try {
    const res = await fetch(url);
    const data = await res.json();
    displayData(data);
  } catch (err) {
    console.error(err);
  }
}
```

### ALLONS PLUS LOIN

#### Requêtes en Parallèle

Si vous devez faire plusieurs requêtes **indépendantes**, utilisez `Promise.all()` :

```javascript
async function loadMultipleData() {
  try {
    // Lancer les 3 requêtes en parallèle
    const [quotes, cats, recipes] = await Promise.all([
      fetch('https://api.quotable.io/quotes/random').then(r => r.json()),
      fetch('https://api.thecatapi.com/v1/images/search?limit=3').then(r => r.json()),
      fetch('https://www.themealdb.com/api/json/v1/1/random.php').then(r => r.json())
    ]);

    console.log('Tout est chargé !', quotes, cats, recipes);
  } catch (error) {
    console.error('Erreur:', error);
  }
}
```

**Avantage :** Les 3 requêtes se font **en même temps**, pas l'une après l'autre. Beaucoup plus rapide !

#### Top-Level Await (Modern)

Dans les modules JavaScript modernes, on peut utiliser `await` directement (sans fonction async) :

```javascript
// Dans un module (.mjs ou <script type="module">)
const response = await fetch(url);
const data = await response.json();
console.log(data);
```

On verra ça demain avec l'architecture en modules.

---

## 3 - Formulaires & preventDefault

### L'essentiel

#### Le Problème avec les Formulaires

Par défaut, quand on soumet un formulaire (`<form>`), la page **se recharge**.

**Testez :**

```html
<form>
  <input type="text" placeholder="Rechercher...">
  <button type="submit">Chercher</button>
</form>
```

Cliquez sur "Chercher" → La page se recharge. Pas bon pour une SPA !

#### La Solution : event.preventDefault()

Pour empêcher le comportement par défaut :

```javascript
const form = document.querySelector('form');

form.addEventListener('submit', (event) => {
  event.preventDefault();  // Empêche le rechargement !
  console.log('Formulaire soumis sans rechargement');
});
```

#### Récupérer la Valeur d'un Input

```javascript
form.addEventListener('submit', (event) => {
  event.preventDefault();

  const input = document.querySelector('input');
  const searchTerm = input.value;

  console.log('Recherche pour:', searchTerm);
});
```

#### Exemple Complet : Search Bar

```html
<form id="search-form">
  <input type="text" id="search-input" placeholder="Rechercher un film...">
  <button type="submit">Chercher</button>
</form>

<div id="results"></div>
```

```javascript
const form = document.querySelector('#search-form');
const searchInput = document.querySelector('#search-input');

form.addEventListener('submit', async (event) => {
  event.preventDefault();

  const query = searchInput.value.trim();

  if (query === '') {
    alert('Entrez un terme de recherche');
    return;
  }

  console.log('Recherche pour:', query);
  // Ici on fera un fetch avec le terme de recherche
});
```

#### Validation Basique

Vérifier que l'input n'est pas vide :

```javascript
if (query === '' || query.length < 3) {
  alert('Entrez au moins 3 caractères');
  return;
}
```

### ALLONS PLUS LOIN

#### Nettoyer l'Input après Submit

```javascript
form.addEventListener('submit', async (event) => {
  event.preventDefault();
  const query = searchInput.value;

  // Faire quelque chose avec query...

  searchInput.value = '';  // Vider l'input
  searchInput.focus();     // Remettre le focus
});
```

#### Enter vs Click

Avec un formulaire, les deux fonctionnent :
- Cliquer sur le bouton `submit`
- Appuyer sur `Enter` dans l'input

Pas besoin de gérer les deux séparément !

---

## 4 - LocalStorage : Sauvegarder des Données

### L'essentiel

#### Qu'est-ce que localStorage ?

`localStorage` permet de **sauvegarder des données dans le navigateur**, même après avoir fermé la page.

**Cas d'usage :**

- Favoris
- Préférences utilisateur
- Panier d'achat
- Thème clair/sombre

**Limite :** Environ 5-10 MB par domaine (largement suffisant pour des favoris).

#### Les 4 Méthodes Principales

```javascript
// 1. Sauvegarder une valeur
localStorage.setItem('key', 'value');

// 2. Récupérer une valeur
const value = localStorage.getItem('key');

// 3. Supprimer une valeur
localStorage.removeItem('key');

// 4. Tout effacer
localStorage.clear();
```

#### Exemple Simple

```javascript
// Sauvegarder un nom
localStorage.setItem('username', 'Alice');

// Récupérer
const name = localStorage.getItem('username');
console.log(name);  // "Alice"

// Supprimer
localStorage.removeItem('username');
```

**Testez dans la console du navigateur !**

#### Sauvegarder des Objets/Arrays

localStorage ne stocke **que des strings**. Pour sauvegarder des objets, utiliser `JSON.stringify()` et `JSON.parse()`.

**Sauvegarder un array :**

```javascript
const favorites = ['Film1', 'Film2', 'Film3'];

// Convertir en string
const favoritesString = JSON.stringify(favorites);
localStorage.setItem('favorites', favoritesString);
```

**Récupérer l'array :**

```javascript
const favoritesString = localStorage.getItem('favorites');

// Convertir en objet JavaScript
const favorites = JSON.parse(favoritesString);
console.log(favorites);  // ['Film1', 'Film2', 'Film3']
```

#### Pattern Complet : Gérer des Favoris

```javascript
// Récupérer les favoris existants (ou array vide si rien)
function getFavorites() {
  const favoritesString = localStorage.getItem('favorites');
  return favoritesString ? JSON.parse(favoritesString) : [];
}

// Ajouter un favori
function addFavorite(movieId) {
  const favorites = getFavorites();

  if (!favorites.includes(movieId)) {
    favorites.push(movieId);
    localStorage.setItem('favorites', JSON.stringify(favorites));
    console.log('Ajouté aux favoris !');
  }
}

// Retirer un favori
function removeFavorite(movieId) {
  let favorites = getFavorites();
  favorites = favorites.filter(id => id !== movieId);
  localStorage.setItem('favorites', JSON.stringify(favorites));
  console.log('Retiré des favoris');
}

// Vérifier si un film est en favoris
function isFavorite(movieId) {
  const favorites = getFavorites();
  return favorites.includes(movieId);
}
```

#### Tester localStorage

Dans la console du navigateur :
1. F12 → Application (Chrome) ou Stockage (Firefox)
2. LocalStorage → Votre domaine
3. Vous voyez toutes vos données sauvegardées !

### ALLONS PLUS LOIN

#### SessionStorage vs localStorage

**localStorage :** Données persistent même après fermeture du navigateur

**sessionStorage :** Données supprimées à la fermeture de l'onglet

```javascript
// Utilisation identique
sessionStorage.setItem('key', 'value');
const value = sessionStorage.getItem('key');
```

#### Écouter les Changements

```javascript
window.addEventListener('storage', (event) => {
  console.log('LocalStorage a changé:', event.key, event.newValue);
});
```

Utile si vous avez plusieurs onglets ouverts !

---

## 5 - Introduction Projet Netflix

### L'essentiel

#### Présentation du Projet Final

**En 2 jours, vous allez créer Netflex : votre mini-Netflix !**

**Aujourd'hui (Jour 2) :**

- Un repository contenant:
- Search bar fonctionnelle
- Afficher des films en grille (affiches + infos)
- Styling Netflix-like

**Demain (Jour 3) :**

- Modal avec détails du film
- Trailers YouTube
- Système de favoris
- Polish & responsive

#### L'API TMDB (The Movie Database)

**TMDB** est l'API de référence pour les données de films :

- Utilisée par de vraies applications
- Gratuite (avec inscription)
- Très bien documentée
- Données riches : affiches, trailers, synopsis, etc.

🔗 [Site TMDB](https://www.themoviedb.org)
🔗 [Documentation API](https://developer.themoviedb.org/docs/getting-started)

#### Créer Votre Compte TMDB

**⚠️ Important :** Vous devez faire ça **maintenant** (sur un desktop, pas mobile).

**Étapes :**

1. **Créer un compte**
   - Aller sur https://www.themoviedb.org/signup
   - Email, username, password
   - Confirmer par email

2. **Demander une clé API**
   - Se connecter
   - Cliquer sur votre avatar (en haut à droite) → Settings
   - Dans le menu de gauche → API
   - Cliquer sur "Create" ou "Request an API Key"
   - Choisir "Developer"
   - Accepter les conditions
   - Remplir le formulaire (mettez ce que vous voulez, c'est juste informatif)

3. **Copier votre clé**
   - Vous obtenez une "API Key (v3 auth)"
   - **Copiez-la** (vous allez l'utiliser dans votre code)

**Temps estimé :** 5-10 minutes maximum

**Si vous bloquez :** Levez la main, on vous aide !

#### Tester Votre Clé API

Dans la console du navigateur :

```javascript
const API_KEY = 'VOTRE_CLE_ICI';  // Remplacez par votre vraie clé
const url = `https://api.themoviedb.org/3/movie/550?api_key=${API_KEY}`;

fetch(url)
  .then(res => res.json())
  .then(data => console.log(data))
```

**Résultat attendu :** Un objet avec les infos du film "Fight Club" (id: 550).

Si ça marche, vous êtes prêts ! 🎉

#### Endpoints Principaux

**Base URL :** `https://api.themoviedb.org/3`
**Image Base URL :** `https://image.tmdb.org/t/p/w500`

Grâce à leur documentation, trouvez comment

1. Rechercher des films
2. Obtenir les films populaires
3. Obtenir les détails d'un film (avec vidéos)

#### Fonctionnalités Minimales vs Extensions

**Ce que TOUT LE MONDE doit avoir (Jour 2 fin) :**

✅ **Fonctionnalités Minimales :**
- Search bar fonctionnelle
- Afficher résultats en grille (affiches + titres + notes)
- Loading state ("Recherche en cours...")
- Message si aucun résultat
- Design propre et lisible

**Extensions pour aller plus loin :**

**Niveau 1 :**
- Films populaires affichés au chargement (avant recherche)
- Afficher la date de sortie et synopsis court
- Hover effect sur les cards
- Loading spinner animé

**Niveau 2 :**
- Filtrer par année de sortie
- Pagination (bouton "Charger plus")
- Afficher le nombre de résultats trouvés
- Vider la recherche (bouton X)

**Niveau 3 :**
- Plusieurs pages (Accueil / Recherche / Trending)
- Catégories (Action, Comédie, etc.)
- Tri (par note, par date, etc.)
- Animations avancées

### ALLONS PLUS LOIN

#### Rate Limiting

TMDB est très généreux, mais évitez de spammer :
- Ne faites pas 100 requêtes par seconde
- Utilisez debouncing pour la search bar (on verra ça demain)

#### Attribution

TMDB demande d'afficher leur logo quelque part sur votre site. Ajoutez dans le footer :

```html
<footer>
  <p>Données fournies par <a href="https://www.themoviedb.org">TMDB</a></p>
</footer>
```

---

## Récapitulatif Jour 2

### Ce qu'on a appris aujourd'hui

1. **Async/Await** : Syntaxe moderne pour gérer l'asynchrone
2. **Formulaires** : `event.preventDefault()` pour empêcher le rechargement
3. **LocalStorage** : Sauvegarder des données dans le navigateur
4. **API TMDB** : Inscription, clé API, endpoints
5. **Projet Netflix Phase 1** : Search bar + grille de films

### Points d'attention pour demain

- **Finir la Phase 1** si vous n'avez pas terminé
- **Expérimenter** : Essayez d'ajouter les fonctionnalités Niveau 1
- **Préparez-vous** : Demain on ajoute modals, trailers YouTube, et favoris !

### Ressources pour Aller Plus Loin

- [Documentation TMDB](https://developer.themoviedb.org/docs)
- [MDN : Async/Await](https://developer.mozilla.org/fr/docs/Learn/JavaScript/Asynchronous/Async_await)
- [MDN : LocalStorage](https://developer.mozilla.org/fr/docs/Web/API/Window/localStorage)

**Bravo pour cette deuxième journée ! 🎉**

**Demain : Modals, Trailers YouTube, et Favoris !** 🍿
