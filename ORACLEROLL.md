# OracleRoll — Point sur le projet

> Application web hors-ligne de lancer de dés et oracles pour jeu de rôle solo ou improvisé. Cible : tablette Android.

---

## 1. Le projet initial

OracleRoll est une application web **100 % statique** (HTML/CSS/JS vanilla, zéro dépendance, zéro serveur). Elle est conçue comme un **toolkit de table** pour les joueurs de JDR solo ou en groupe, fonctionnant entièrement dans le navigateur avec persistence dans `localStorage`.

**Principes fondateurs :**
- Aucun build, aucun framework, aucun outil de compilation — tout est dans le navigateur
- Architecture modulaire : un fichier HTML + 7 CSS + 14 JS, tous en chemins relatifs
- Données persistées localement (univers, tables, decks, progression, battle map)
- Interface pensée d'abord pour la **tablette** (et non le PC)

---

## 2. Les modules disponibles

L'application compte **5 onglets** :

### Dés (`dice/`)
- D4, D6, D8, D10, D12, D20, D100 + dé personnalisé
- 5 modes : Addition, Séparation, Soustraction, Avantage, Désavantage
- Animation 3D de lancer avec effet de flash

### Oracle (`oracle/`)
- Système d'**univers** (SF, Fantasy, Pirate, Générique + univers personnalisés)
- Chaque univers contient des **tables** à colonnes (Action, Lieu, Personnage...)
- 3 modes de tirage : toutes colonnes, colonne unique, mixte (tirage indépendant par colonne)
- Animation slot-machine, favoris, bascule rapide entre tables

### Tirage (`deck/`)
- **Jeu de 52 cartes + 2 Jokers** (significations par couleur/valeur)
- **Decks personnalisés** (créés dans l'éditeur de contenu)
- **Decks image GitHub** (cartes visuelles hébergées sur GitHub, format `CARDS_SPECS`)
- Retournement de carte (flip), dessin avec pioche aléatoire

### Battle Map (`battlemap/`)
- Grille NxN (5, 10 ou 15) avec étiquettes colonnes/lignes
- **Dessin libre** sur canvas (6 couleurs, gomme, remplissage)
- **Pions** (jetons) avec 3 formes et couleurs, déplaçables
- Terrain variable par cellule (herbe, eau, glace, rocher...)
- Export PNG (3x), export/import JSON

### Contenu (`editor/`)
- Création/suppression d'**univers**, **tables** et **decks personnalisés**
- Interface de création de grille (nom, dé, colonnes)
- Import/export JSON de toutes les données
- Transfert entre appareils via fichier

### Barre de progression
- Fixée en bas de l'écran, valeur 1-20, persistée en localStorage

---

## 3. Relation entre l'éditeur de cartes et OracleRoll

### oracle-card-editor.html (application séparée)

C'est un **outil de conception de cartes visuelles** — un éditeur standalone (fichier unique, ~1250 lignes) qui fonctionne de manière totalement indépendante d'OracleRoll.

**Ce qu'il fait :**
- Crée des images de cartes au format **660×1140 px**
- Deux faces par carte (recto/verso), chacune avec : image, nom, description, icônes type/catégorie
- Rendu Canvas avec ornaments dorés, frises, médaillons
- Gestion de decks (création, sélection, ajout/suppression de cartes)
- Filtres image (luminosité, contraste, saturation), recadrage

**Comment il s'articule avec OracleRoll :**

```
oracle-card-editor.html          GitHub           OracleRoll
┌─────────────────┐         ┌──────────┐      ┌─────────────────┐
│  Conception      │ export  │  Hébergé  │ load │  Tirage         │
│  des cartes      │────────▶│  en JSON  │◀─────│  (onglet cartes)│
│  (fichier local) │  JSON   │  public   │ URL  │  (navigateur)   │
└─────────────────┘         └──────────┘      └─────────────────┘
```

1. **Conception** : on crée les cartes dans l'éditeur (images, textes, icônes)
2. **Export** : l'éditeur génère un fichier JSON au format `CARDS_SPECS` contenant les images PNG encodées en base64
3. **Hébergement** : le JSON est poussé sur un dépôt GitHub public
4. **Chargement** : dans OracleRoll, on colle l'URL GitHub dans l'onglet Tirage → le deck se charge et les cartes s'affichent avec animation de retournement

**Format d'échange (`CARDS_SPECS`) :**
```json
{
  "format": "CARDS_SPECS",
  "deck": {
    "name": "Nom du deck",
    "cards": [
      { "id": "card-1", "name": "Nom de la carte", "image": "data:image/png;base64,..." }
    ]
  }
}
```

---

## 4. Intégration GitHub

### Rôle de GitHub dans l'écosystème

GitHub remplit **deux fonctions** distinctes :

| Fonction | Usage |
|---|---|
| **Hébergement des decks image** | Les JSON `CARDS_SPECS`exportés par l'éditeur de cartes sont stockés dans des dépôts publics et chargés par OracleRoll via URL |
| **GitHub Pages (OracleRoll)** | L'application elle-même est servie via `https://Foxyte17.github.io/OracleRoll/` pour résoudre le problème d'affichage sur tablette Android (les content:// URI d'Android empêchent le chargement des CSS/JS en local) |

### Pourquoi GitHub Pages ?

Sur Android, le gestionnaire de fichiers ouvre les `.html` via un **URI `content://`** qui ne permet pas de résoudre les chemins relatifs vers `css/` et `js/`. GitHub Pages sert les fichiers en HTTP, ce qui contourne完全ment ce problème.

### Workflow de mise à jour

```bash
git add .
git commit -m "Description du changement"
git push
```

Le site est mis à jour automatiquement en ~30 secondes.

---

## 5. Feuille de route

### En cours

- [x] Diagnostic et correction du bug d'affichage tablette (switch vers GitHub Pages)
- [x] Renommage `oracleroll.html` → `index.html` pour GitHub Pages

### Court terme

- [ ] **Corriger le design des cartes** : affiner les visuels dans l'éditeur (typographie, alignements, lisibilité des descriptions, harmonie des couleurs)
- [ ] **Améliorer l'ergonomie globale** : vérifier la taille des zones tactiles sur tablette, l'espacement des boutons, la lisibilité des textes en portrait comme en paysage

### Moyen terme

- [ ] **Construire un premier deck complet** : créer un deck de cartes visuelles fonctionnel (test de validation du workflow éditeur → GitHub → OracleRoll)
- [ ] **Tester en conditions réelles** : session de jeu solo sur tablette avec le deck, les dés et l'oracle — identifier les frictions d'usage

### Pistes d'amélioration à considérer

- [ ] **Deck image hors-ligne** : actuellement les decks GitHub ne sont pas persistés (session only) — envisager un cache local pour les utiliser sans connexion
- [ ] **Documentation utilisateur** : guide rapide de prise en main pour les joueurs (comment créer un univers, lancer des dés, charger un deck)
