# Netflix Phase 2 : Finalisation

## Objectif Aujourd'hui

Ajouter à votre projet d'hier :

1. ✅ Meilleure architecture de code
2. ✅ Modal qui s'ouvre au clic sur un film  
   ✅ Détails du film dans la modal (synopsis, date, genres)
3. ✅ Trailer YouTube dans la modal
4. ✅ Optimisation de la barre de recherche
5. ✅ Système de favoris avec localStorage  
   ✅ Section "Mes Favoris"
6. ✅ Polish : CSS Netflix-like, responsive

---

## Etape 1 - Architecture Code (Modules)

### Pourquoi Organiser le Code ?

Actuellement, tout est dans `script.js`. Ça marche, mais :

- Difficile à maintenir
- Difficile à tester
- Difficile à réutiliser

**Bonne pratique :** Séparer les responsabilités.

### Structure en 3 Fichiers

```
flix-search/
├── index.html
├── style.css
├── js/
│   ├── api.js       # Toutes les fonctions fetch
│   ├── ui.js        # Manipulation DOM
│   └── app.js       # Logique principale
```

---

## Etape 2 - Modales

> Faire une **fenêtre popup** qui s'affiche par-dessus le contenu principal.

**Exemples :**

- Détails d'un produit (e-commerce)
- Détails d'un film (Netflix)
- Formulaire de connexion

#### HTML de la Modal

Ajouter dans `index.html` (avant `</body>`) :

```html
<div id="modal" class="modal hidden">
  <div class="modal-overlay"></div>
  <div class="modal-content">
    <button id="close-modal" class="close-btn">&times;</button>
    <div id="modal-body"></div>
  </div>
</div>
```

**Structure CSS :**

- `.modal` : Conteneur principal
- `.modal-overlay` : Fond sombre transparent
- `.modal-content` : Boîte blanche centrée
- `#modal-body` : Où on met le contenu

### JavaScript : Ouvrir/Fermer le Modal

```javascript
const modal = document.querySelector("#modal");
const closeBtn = document.querySelector("#close-modal");
const modalBody = document.querySelector("#modal-body");

function openModal(content) {
  modalBody.innerHTML = content;
  modal.classList.remove("hidden");
}

function closeModal() {
  modal.classList.add("hidden");
  modalBody.innerHTML = "";
}

// Fermer avec le bouton X
closeBtn.addEventListener("click", closeModal);

// Fermer en cliquant sur l'overlay
modal.addEventListener("click", (event) => {
  if (event.target.classList.contains("modal-overlay")) {
    closeModal();
  }
});

// Fermer avec la touche Escape
document.addEventListener("keydown", (event) => {
  if (event.key === "Escape" && !modal.classList.contains("hidden")) {
    closeModal();
  }
});
```

---

## Etape 3 - Intégration YouTube (Trailers)

#### Récupérer les Vidéos d'un Film

TMDB retourne les trailers YouTube dans l'endpoint des détails :

```javascript
async function getMovieDetails(movieId) {
  const url = `${BASE_URL}/movie/${movieId}?api_key=${API_KEY}&append_to_response=videos&language=fr-FR`;

  try {
    const response = await fetch(url);
    const movie = await response.json();
    return movie;
  } catch (error) {
    console.error("Erreur:", error);
    return null;
  }
}
```

**Structure de la réponse :**

```javascript
{
  "id": 550,
  "title": "Fight Club",
  "overview": "...",
  "videos": {
    "results": [
      {
        "key": "dQw4w9WgXcQ",  // YouTube video ID
        "name": "Official Trailer",
        "site": "YouTube",
        "type": "Trailer",
        "official": true
      }
    ]
  }
}
```

#### Filtrer pour Avoir le Trailer

```javascript
function getTrailer(movie) {
  const videos = movie.videos.results;

  // Chercher un trailer officiel sur YouTube
  const trailer = videos.find(
    (video) => video.type === "Trailer" && video.site === "YouTube"
  );

  return trailer;
}
```

#### Embed YouTube avec iframe

**Construire l'URL YouTube :**

```javascript
const trailer = getTrailer(movie);

if (trailer) {
  const youtubeUrl = `https://www.youtube.com/embed/${trailer.key}`;
  console.log(youtubeUrl);
}
```

**Créer l'iframe :**

```javascript
if (trailer) {
  const iframeHTML = `
    <div class="trailer-container">
      <h3>Bande-annonce</h3>
      <iframe
        width="100%"
        height="400"
        src="https://www.youtube.com/embed/${trailer.key}"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen>
      </iframe>
    </div>
  `;
}
```

**CSS pour l'iframe :**

```css
.trailer-container {
  margin-top: 20px;
}

.trailer-container iframe {
  border-radius: 8px;
}
```

#### Autoplay

Faire que la vidéo démarre automatiquement.

#### Responsive 16:9

Garder le ratio 16:9 sur mobile.

---

## Etape 4 - Optimisation de la SearchBar

> Les Patterns de Professionnels

### Debouncing : Optimiser la Search Bar

**Le problème :**

Si vous tapez "Matrix" dans la search bar, 6 requêtes API sont envoyées :

- M
- Ma
- Mat
- Matr
- Matri
- Matrix

**Gaspillage de ressources** et risque de rate limiting.

**La solution : Debouncing**

Attendre que l'utilisateur **arrête de taper** pendant X millisecondes avant de lancer la requête.

**Implémentation :**

```javascript
let debounceTimer;

searchInput.addEventListener("input", (event) => {
  const query = event.target.value.trim();

  // Annuler le timer précédent
  clearTimeout(debounceTimer);

  // Si moins de 3 caractères, ne rien faire
  if (query.length < 3) {
    moviesGrid.innerHTML = "";
    return;
  }

  // Lancer un nouveau timer
  debounceTimer = setTimeout(() => {
    searchMovies(query);
  }, 500); // 500ms après la dernière frappe
});
```

**Explications :**

- `setTimeout()` : Exécute une fonction après X millisecondes
- `clearTimeout()` : Annule le timer précédent
- Résultat : La requête n'est lancée que 500ms après que l'utilisateur a **fini** de taper

**Testez !** Tapez "Matrix" dans votre search bar. Vous verrez qu'une seule requête est lancée (au lieu de 6).

🔗 [Aide : Debouncing expliqué](https://www.freecodecamp.org/news/javascript-debounce-example/)

#### Error Handling Avancé

**Gérer différents types d'erreurs :**

```javascript
async function searchMovies(query) {
  try {
    moviesGrid.innerHTML =
      '<p class="loading">🔄 Recherche en cours...</p>';

    const url = `${BASE_URL}/search/movie?api_key=${API_KEY}&query=${query}`;
    const response = await fetch(url);

    // Erreur HTTP
    if (!response.ok) {
      if (response.status === 401) {
        throw new Error("Clé API invalide");
      } else if (response.status === 404) {
        throw new Error("API non trouvée");
      } else {
        throw new Error(`Erreur ${response.status}`);
      }
    }

    const data = await response.json();
    displayMovies(data.results);
  } catch (error) {
    console.error("Erreur:", error);

    // Message user-friendly
    moviesGrid.innerHTML = `
      <p class="error">❌ ${error.message}</p>
      <p>Veuillez réessayer dans quelques instants.</p>
    `;
  }
}
```

#### Loading States Avancés

**Spinner animé CSS :**

```css
.loading-spinner {
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid #e50914;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
  margin: 40px auto;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

**Utilisation :**

```javascript
moviesGrid.innerHTML = '<div class="loading-spinner"></div>';
```

#### Retry Logic

Réessayer automatiquement si une requête échoue :

```javascript
async function fetchWithRetry(url, retries = 3) {
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(url);
      if (response.ok) {
        return await response.json();
      }
    } catch (error) {
      if (i === retries - 1) throw error;
      await new Promise((resolve) =>
        setTimeout(resolve, 1000 * (i + 1))
      );
    }
  }
}
```

#### Throttling

Alternative au debouncing : Limiter à X requêtes par seconde.

## Etape 5 - Système de Favoris (localStorage)

## Etape 6 - Pimper Netflex

> Extensions du projet - CSS Netflix-like, responsive...

- Scroll Lock  
  Quand le modal est ouvert, empêcher le scroll de la page
- Animations  
  Ajouter des transitions CSS

Si ce n'est pas déjà fait :

- Films populaires affichés au chargement (avant recherche)
- Afficher la date de sortie et synopsis court
- Hover effect sur les cards
- Loading spinner animé

- Filtrer par année de sortie
- Pagination (bouton "Charger plus")
- Afficher le nombre de résultats trouvés
- Vider la recherche (bouton X)

- Plusieurs pages (Accueil / Recherche / Trending)
- Catégories (Action, Comédie, etc.)
- Tri (par note, par date, etc.)
- Animations avancées
