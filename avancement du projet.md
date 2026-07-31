# OracleRollLight — Avancement du projet

> Application oracle personnelle pour jeu de rôle solo/improvisation : dés, tables d'oracle, tirage de cartes. Cible prioritaire : **tablette** (avant le PC). Dossier de travail : `OracleRollLightTESTCOPIE`.

## 1. État actuel

- **Architecture modulaire** : `index.html` (475 lignes, zéro code inline) + 7 fichiers CSS + 14 fichiers JS, tous en chemins relatifs.
- **Ordre de chargement des scripts : critique, ne jamais modifier** (lignes 460-473 d'`index.html`) : `utils.js` → `storage.js` → `navigation.js` → `progress-bar.js` → `dice-engine.js` → `dice-ui.js` → `oracle-engine.js` → `oracle-ui.js` → `deck-engine.js` → `deck-ui.js` → `editor.js` → `battlemap-engine.js` → `battlemap-ui.js` → `app.js`.
- **Non responsive** : aucune media query dans `css/`, layout fixe `.app { max-width: 720px }`. Interface à adapter en priorité pour tablette (portrait et paysage).
- **Résolu — affichage tablette** : le CSS/JS ne se chargeait pas en local sur tablette (Android ouvre via `content://` au lieu de `file:///`, les chemins relatifs ne se résolvent pas). Solution : **GitHub Pages** pour OracleRoll. Le fichier a été renommé `oracleroll.html` → `index.html` (servi automatiquement par Pages). URL : `https://Foxyte17.github.io/OracleRoll/`.

## 2. Fonctionnalités en place

- **Dés** : D4→D100 + dé perso, modes Addition/Soustraction/Séparation/Avantage/Désavantage, animation.
- **Oracle** : univers → tables → détail, tirages, favoris.
- **Tirage** : 52 cartes + 2 Jokers (significations par couleur), decks personnalisés, **decks image Card Editor via GitHub**.
- **Battle Map** : grille, pions, dessin canvas, export PNG/JSON.
- **Contenu (éditeur)** : univers, tables, decks personnalisés, export/import JSON.
- **Barre de progression** fixe en bas d'écran (échelle 1-20, persiste en localStorage).

## 3. Dernier chantier : Decks GitHub (format `CARDS_SPECS`)

Chargement de decks Card Editor par URL de manifest GitHub dans l'onglet Tirage.

- **Format du manifest** : `{ "format": "CARDS_SPECS", "deck": { "name", "cards": [{ id, name, image }] } }`. Champ `format` validé en premier.
- **Conversion d'URL automatique** : `github.com/{o}/{r}/blob/{b}/{p}` et `.../raw/...` → `raw.githubusercontent.com/{o}/{r}/{b}/{p}`.
- **GitHub strict** : toute URL non-GitHub → erreur `bad-url`.
- **7 erreurs distinctes** : `empty-url`, `bad-url`, `fetch-failed`, `invalid-json`, `bad-format`, `invalid-structure`, `instance-fields` (champ de Card Instance interdit dans le manifest).
- **Décisions validées** :
  - **Pas de persistance runtime** : deck et état de partie en mémoire uniquement (session), comme les decks actuels.
  - Carte = **1 PNG 660×1140** contenant les deux faces (recto = moitié haute, verso = moitié basse à l'envers).
  - Affichage **en place** dans le panneau Tirage (pas d'overlay), format `min(80vw, 420px)`, `aspect-ratio: 660/1140`.
  - Retournement = rotation 180° : bouton **"Pivoter"** + clic sur la carte.
- **Conformité vérifiée** : périmètre respecté (4 fichiers), plus d'`id` injecté dans les handlers (index `data-idx`), ids garantis uniques à la normalisation, étape 6 (carte entière + Pivoter) livrée.

**Fichiers de la mission** (seuls modifiables pour ce chantier) :
- `js/modules/deck/deck-engine.js`, `js/modules/deck/deck-ui.js`, `index.html`, `css/modules/deck.css`.
- **Interdits** : `js/core/storage.js`, `js/app.js`, `js/modules/editor/editor.js`, l'ordre des `<script>`, les autres modules/CSS.

## 4. En attente / prochaines étapes

1. **Passage responsive** (priorité) : media queries, toute l'interface lisible sur tablette portrait et paysage.
2. Vérification du retour builder sur le prompt responsive.

## 5. Règles de travail

- **OracleRoll → GitHub Pages** : pour la tablette, OracleRoll est servie via GitHub Pages (`https://Foxyte17.github.io/OracleRoll/`). Tout push sur la branche `main` met à jour le site. Ne pas confondre avec les autres projets du workspace qui restent en local.
- Ne jamais modifier l'ordre de chargement des scripts ni renommer des fonctions sans tout tester.
- Tester dans un navigateur après chaque changement (les 5 onglets).
- Proposer les choix avant de coder pour les décisions structurantes ; privilégier les solutions simples et robustes.
