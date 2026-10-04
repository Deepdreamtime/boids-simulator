# 🐦 Nuée d'Oiseaux - Simulation Boids Psychédélique

> Une simulation de nuées d'oiseaux (Boids) ultra-colorée, interactive et optimisée. 400 oiseaux à 60fps, avec liens neuronaux, prédateurs multiples et modes chaos.

[Boids](https://img.shields.io/badge/Boids-Simulation-00d9ff?style=for-the-badge)
[JavaScript](https://img.shields.io/badge/Vanilla_JS-No_Framework-ffcc00?style=flat-square)
[Perf](https://img.shields.io/badge/400_boids-60fps-00ff88?style=flat-square)
[License](https://img.shields.io/badge/license-MIT-purple?style=flat-square)

**[→ Démo live](https://...)** · [Signaler un bug](../../issues) · [Proposer une feature](../../issues)

---

### ✨ Aperçu

Une implémentation moderne de l'algorithme **Boids** de Craig Reynolds (1987), réinventée en version festival psychédélique.

Pas de librairie. Un seul fichier HTML. Juste des maths, du canvas et beaucoup de couleurs.

---

### 🎮 Fonctionnalités

#### 🎨 Visuels
- **6 palettes de couleurs** : Néon, Arc-en-ciel, Vitesse (heat-map), Sunset, Forêt, Acide
- **4 fonds animés** : Nuit, Aurore, Océan profond, Néon City avec dégradés qui respirent
- **Rendu glow** avec `shadowBlur` et traînées comètes pour chaque oiseau
- Taille et teinte légèrement variable pour un effet organique

#### 🔗 Liens entre boids
- Visualisation du réseau de voisinage en temps réel
- Réglage distance + opacité
- Mode **Constellation** : ne relie que le plus proche voisin (effet migrateur minimaliste)

#### 👹 Prédateurs & Comportements
- **3 types de prédateurs** (0 à 8 simultanés) :
  - `FAUCON` : chasse active, explosion de particules à l'impact
  - `FANTÔME` : effraie sans tuer, traînée blanche
  - `TROU NOIR` : attraction gravitationnelle, orbite
- **Boutons Chaos** :
  - 🌀 Vortex - tornade centrale de 5s
  - 💥 Panique - explosion radiale puis regroupement
  - ✨ Murmuration - onde de couleur façon étourneaux
  - 🍯 Nourriture - dépose des orbes lumineux au clic
- **Leader doré** : un boid boss que toute la nuée suit
- **Obstacles** : 3 rochers avec évitement
- **Easter eggs** : étincelles à haute vitesse, grossissement après repas

#### 🎛️ Contrôles Boids complets
| Paramètre | Description |
| :--- | :--- |
| Nombre d'oiseaux | 10 → 400 |
| Vitesse max | Vitesse de croisière |
| Force max | Agilité / réactivité en virage |
| Rayon de perception | Distance à laquelle ils voient leurs voisins |
| Rayon de répulsion | Bulle personnelle |
| Poids Attraction (Cohésion) | Se rapprocher du centre du groupe |
| Poids Alignement | Voler dans la même direction |
| Poids Répulsion (Séparation) | Éviter les collisions |

#### ⚡ Performance
- **Grille spatiale (Spatial Hashing)** : O(n²) → O(n). Chaque boid ne teste que sa cellule + 8 voisines.
- Pas de `sqrt()` inutile, calculs en `dist²`
- Optimisé pour 400 boids @ 60fps même avec les liens

---

### 🚀 Installation & Utilisation

C'est un seul fichier, pas de build.

```bash
git clone https://github.com/ton-user/nuee-oiseaux.git
cd nuee-oiseaux
# Ouvre index.html dans ton navigateur
open index.html
```

Ou simplement drag & drop le fichier `.html` dans Chrome/Firefox.

#### Presets 1-clic
- **Calme** : vol lent, très groupé
- **Essaim nerveux** : vitesse 7, répulsion forte
- **Rave** : Acide + liens + traînées
- **Murmuration d'étourneaux** : le classique hypnotique
- **Chasse** : 3 faucons + panique

---

### 🧠 Comment ça marche ?

Chaque boid applique 3 règles :

1.  **Séparation** `steer += (pos - voisin.pos) / distance` si `distance < rayonRépulsion`
2.  **Alignement** `steer += voisin.vel` moyenné sur le voisinage
3.  **Cohésion** `steer += (centreVoisins - pos)`

```js
velocity += separation * poidsRépulsion
         += alignment * poidsAlignement
         += cohesion * poidsAttraction
velocity = limit(velocity, vitesseMax)
```

On ajoute ensuite prédateurs, obstacles, nourriture et vortex comme des forces supplémentaires.

**Optimisation grille :**
```js
// Au lieu de checker 400*400 = 160k paires
// On divise l'écran en cellules de 80px
// Chaque boid ne check que ~15 voisins
cellX = floor(x / cellSize)
cellY = floor(y / cellSize)
voisins = grid[cellX-1 to cellX+1][cellY-1 to cellY+1]
```

---

### 📁 Structure

```
.
├── index.html          # Tout est dedans (HTML/CSS/JS)
├── README.md
└── LICENSE
```

Le code est découpé en 3 classes :
- `Boid` : position, vélocité, update, flock, comportements
- `Predator` : IA de chasse
- `Flock` : grille spatiale, gestion du canvas, particules

---

### 🎨 Personnalisation

Tu veux changer les couleurs ? Cherche `PALETTES` dans le JS :

```js
const PALETTES = {
  neon: (boid) => `hsl(${180 + boid.id % 60}, 100%, 60%)`,
  rainbow: (boid) => `hsl(${(boid.angle * 180/Math.PI) % 360}, 100%, 60%)`,
  // ajoute la tienne
}
```

Tu veux ajouter un comportement ? Ajoute une force dans `Boid.update()` :

```js
let wind = createVector(sin(time*0.001)*0.05, 0)
acceleration.add(wind)
```

---

### 🗺️ Roadmap

- [ ] Mode micro : la nuée réagit au son
- [ ] Mode dessin : peindre avec la nuée
- [ ] Export video (MediaRecorder)
- [ ] Multi-nuées avec couleurs rivales
- [ ] Version WebGL pour 2000+ boids

---

### 🤝 Contribuer

Les PR sont les bienvenues ! 

1. Fork
2. `git checkout -b feature/ma-feature-cool`
3. Commit
4. Push + PR

---

### 📄 License

MIT - fais-en ce que tu veux, mais montre-moi ce que tu crées !

---

Fait avec ❤️ et beaucoup de `requestAnimationFrame` à Creuzier-le-Neuf.

> "Si les oiseaux avaient GitHub, ils forkeraient ça."
