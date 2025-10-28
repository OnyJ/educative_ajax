# Cours AJAX - 3 Jours

**Formation intensive pour apprendre AJAX et créer son mini-Netflix**

---

## 📚 Vue d'Ensemble

Ce cours de 3 jours vous emmène de zéro à la création d'une application complète de recherche de films (Netflex - mini-Netflix) en utilisant l'API TMDB.

**Progression :**
- **Jour 1** : Fondations AJAX & Quote/Cat Gallery
- **Jour 2** : Netflix Phase 1 - Search & Grid
- **Jour 3** : Netflix Phase 2 - Modal, Trailers & Favoris

---

## 🎯 Objectifs Pédagogiques

À la fin de ce cours, vous serez capable de :

- ✅ Faire des requêtes HTTP avec l'API Fetch
- ✅ Gérer l'asynchrone (Promesses, async/await)
- ✅ Parser et manipuler des données JSON
- ✅ Créer des interfaces dynamiques (manipulation DOM)
- ✅ Gérer des formulaires et la validation
- ✅ Utiliser localStorage pour sauvegarder des données
- ✅ Créer des modals interactifs
- ✅ Intégrer des vidéos YouTube
- ✅ Organiser votre code en modules
- ✅ Construire une Single Page Application (SPA)

---

## 📂 Structure du Cours

```
educative_ajax/
├── Partie1.md                      # Jour 1 - Fondations
├── Partie2.md                      # Jour 2 - Netflix Phase 1
├── Partie3.md                      # Jour 3 - Netflix Phase 2
├── RAPPORT_VALIDATION_APIS.md      # Validation des APIs utilisées
├── starters/
│   ├── j2-netflix-starter/         # Starter minimaliste J2
│   └── j3-netflix-starter/         # Starter avec structure complète J3
└── README.md                       # Ce fichier
```

---

## 📅 Planning Détaillé

### Jour 1 : Première Conversation avec une API

**Matin (Théorie + Live Coding) :**
- Introduction : Le problème qu'AJAX résout
- HTTP & APIs : Client-Serveur, méthodes, codes de statut
- Première requête avec Fetch
- JSON & Manipulation du DOM

**Après-midi (Pratique) :**
- **Projet Fil Rouge** : Quote & Cat Gallery
  - API Quotable (citations aléatoires)
  - API The Cat (photos de chats)
  - Setup de A à Z, pas de starter fourni

**Bonus :**
- Projet Recipe Finder (TheMealDB)

---

### Jour 2 : Netflix Phase 1 - Search & Display

**Matin (Théorie + Live Coding) :**
- Révisions & Démos J1
- Async/Await : Syntaxe moderne
- Formulaires & preventDefault
- LocalStorage : Sauvegarder des données
- Introduction API TMDB (inscription, clé API)

**Après-midi (Pratique) :**
- **Projet Netflix Phase 1**
  - Search bar fonctionnelle
  - Grille de films avec affiches
  - Loading states

---

### Jour 3 : Netflix Phase 2 - Finalization & Polish

**Matin (Théorie + Live Coding) :**
- Révisions & Démos J2
- Patterns professionnels (debouncing, error handling)
- Modals : Créer et gérer
- Intégration YouTube (trailers)
- Architecture code (modules : api.js, ui.js, app.js)

**Après-midi (Pratique) :**
- **Projet Netflix Phase 2**
  - Modal avec détails du film
  - Trailers YouTube
  - Système de favoris (localStorage)
  - Section "Mes Favoris"
  - Polish & Responsive
  - Starter avec HTML/CSS complets fourni

---

## 🔧 APIs Utilisées

### Jour 1

**1. Quotable API** (Citations)
- URL : `https://api.quotable.io/quotes/random`
- Pas de clé requise
- Documentation : [GitHub](https://github.com/lukePeavey/quotable)

**2. The Cat API** (Photos de Chats)
- URL : `https://api.thecatapi.com/v1/images/search?limit=3`
- Pas de clé requise pour usage basique
- Documentation : [TheCatAPI](https://docs.thecatapi.com)

**3. TheMealDB** (Recettes - Bonus)
- URL : `https://www.themealdb.com/api/json/v1/1/random.php`
- Pas de clé requise
- Documentation : [TheMealDB](https://www.themealdb.com/api.php)

### Jour 2 & 3

**TMDB (The Movie Database)**
- Base URL : `https://api.themoviedb.org/3`
- **Clé API requise** (gratuite, inscription rapide)
- Inscription : [TMDB Signup](https://www.themoviedb.org/signup)
- Documentation : [TMDB Docs](https://developer.themoviedb.org/docs)

**Endpoints principaux :**
- Search : `/search/movie?api_key=KEY&query=matrix`
- Popular : `/movie/popular?api_key=KEY`
- Details : `/movie/550?api_key=KEY&append_to_response=videos`
- Images : `https://image.tmdb.org/t/p/w500/poster_path.jpg`

---

## 💻 Pré-requis

**Connaissances :**
- HTML/CSS de base
- JavaScript fondamental (variables, fonctions, conditions, boucles)
- Manipulation DOM basique (`querySelector`, `addEventListener`)

**Outils nécessaires :**
- VS Code (ou autre éditeur)
- Navigateur moderne (Chrome/Firefox)
- Compte GitHub (pour le rendu)
- Compte TMDB (à créer en J2, gratuit)

**Optionnel mais recommandé :**
- Extension VS Code : Live Server
- Extension VS Code : Prettier

---

## 📦 Starters Fournis

### Jour 2 : Starter Minimal

**Contenu :**
- `index.html` : Structure HTML de base avec TODOs
- `style.css` : Reset CSS + quelques bases
- `script.js` : Configuration API + TODOs

**Objectif :** Ils complètent le HTML (form), CSS (grille, cards), et tout le JavaScript.

### Jour 3 : Starter Complet

**Contenu :**
- `index.html` : Structure HTML complète (header, nav, modal, footer)
- `style.css` : CSS Netflix-like complet et responsive
- `js/api.js` : Structure avec TODOs pour les fonctions fetch
- `js/ui.js` : Structure avec TODOs pour l'affichage
- `js/app.js` : Structure avec TODOs pour la logique

**Objectif :** Focus 100% sur la logique AJAX et fonctionnalités.

---

## 🎯 Fonctionnalités par Niveau

### Fonctionnalités Minimales (Obligatoires)

**Ce que TOUT LE MONDE doit avoir :**

**Jour 1 :**
- [x] Citations aléatoires avec refresh
- [x] Galerie de 3 chats avec refresh
- [x] Design basique propre

**Jour 2 :**
- [x] Search bar fonctionnelle
- [x] Grille de films avec affiches + titres + notes
- [x] Loading state
- [x] Message si aucun résultat

**Jour 3 :**
- [x] Modal au clic sur un film
- [x] Détails (synopsis, date, genres, note)
- [x] Trailer YouTube
- [x] Favoris (localStorage)
- [x] Section "Mes Favoris"
- [x] Responsive (mobile & desktop)

---

### Extensions (Pour Aller Plus Loin)

**Niveau 1 :**
- Films populaires au chargement (avant search)
- Hover effects sur les cards
- Loading spinner animé
- Animations de transition
- Nombre de résultats trouvés

**Niveau 2 :**
- Pagination ("Charger plus")
- Filtrer par année de sortie
- Vider la recherche (bouton X)
- Notification toast au lieu d'alert
- Mode clair/sombre

**Niveau 3 :**
- Plusieurs pages (routing)
- Catégories de films (Action, Comédie, etc.)
- Tri (par note, date, titre)
- Acteurs & équipe technique
- Films similaires
- Watchlist séparée des favoris

---

## 📝 Évaluation

**Critères d'évaluation :**

| Critère | Pondération | Description |
|---------|-------------|-------------|
| Fonctionnalités | 40% | Toutes les features minimales implémentées |
| Code Quality | 30% | Organisation, nommage, comments, async/await |
| UI/UX | 20% | Design Netflix-like, responsive, loading states |
| Documentation | 10% | README clair avec instructions et screenshot |

**Rendu attendu :**
- Repository GitHub public
- Code source complet
- README avec screenshot/GIF
- Déployé sur GitHub Pages (bonus apprécié)

---

## 🛠️ Installation & Lancement

### Jour 1 : Quote & Cat Gallery

```bash
# Créer le dossier
mkdir quote-cat-app
cd quote-cat-app

# Créer les fichiers
touch index.html style.css script.js

# Ouvrir dans VS Code
code .

# Lancer (Live Server ou ouvrir index.html)
```

### Jour 2-3 : Netflex

```bash
# Utiliser le starter fourni
cd starters/j2-netflix-starter  # ou j3-netflix-starter

# Modifier script.js : Remplacer VOTRE_CLE_ICI par votre clé TMDB

# Lancer (Live Server ou ouvrir index.html)
```

---

## 📚 Ressources Complémentaires

**Documentation Officielle :**
- [MDN : Fetch API](https://developer.mozilla.org/fr/docs/Web/API/Fetch_API)
- [MDN : Async/Await](https://developer.mozilla.org/fr/docs/Learn/JavaScript/Asynchronous/Async_await)
- [JavaScript.info](https://javascript.info/)

**Tutoriels Vidéo :**
- [Fireship](https://www.youtube.com/@Fireship) : Concepts rapides
- [Web Dev Simplified](https://www.youtube.com/@WebDevSimplified)
- [FreeCodeCamp](https://www.youtube.com/@freecodecamp)

**Pratiquer :**
- [Frontend Mentor](https://www.frontendmentor.io/)
- [JavaScript30](https://javascript30.com/)
- [APIs Publiques](https://github.com/public-apis/public-apis)

---

## 🎉 Crédits

- **APIs utilisées :** Quotable, The Cat API, TheMealDB, TMDB
- **Inspiration :** Netflix, FreeCodeCamp, Web Dev Simplified
