# Instructions Claude — OracleRoll & Éditeur de cartes

> Document de référence pour tout travail sur l'interface OracleRoll et l'éditeur de cartes.
> L'intégration est terminée (JSON, GitHub, Pages fonctionnent). On se focalise sur les **corrections d'interface**.

---

## 1. Vue d'ensemble

### OracleRoll
Application web **100 % vanilla** (HTML/CSS/JS, zéro framework) de toolkit JDR solo. 5 onglets : Dés, Oracle, Tirage, Battle Map, Contenu. Données en `localStorage`. Servie via **GitHub Pages** : `https://Foxyte17.github.io/OracleRoll/`

### Éditeur de cartes (oracle-card-editor)
Outil séparé de conception de cartes visuelles (660×1140 px). **Fichier découpé en modules** (HTML + JS séparés). Exporte un JSON `CARDS_SPECS` + PNG, hébergés sur GitHub.

### Lien entre les deux
L'éditeur produit des decks → hébergés sur GitHub → chargés dans OracleRoll via URL dans l'onglet Tirage.

---

## 2. État du projet (ce qui fonctionne)

- [x] Affichage tablette via GitHub Pages
- [x] Format CARDS_SPECS fonctionnel
- [x] Chargement de decks GitHub dans OracleRoll
- [x] Export/import JSON
- [x] Tous les modules (dés, oracle, tirage, battle map, contenu)

---

## 3. Ce qui reste à faire (focus actuel)

### Prioritaire
- [ ] **Corrections d'interface** : design des cartes, ergonomie, lisibilité sur tablette

### Secondaire
- [ ] Premier deck complet pour test en conditions réelles
- [ ] Documentation utilisateur

---

## 4. Contraintes techniques

### Pour OracleRoll (index.html)
- Zéro dépendance, zéro build
- Ne jamais modifier l'ordre des `<script>` (lignes 460-473)
- Ne jamais renommer des fonctions sans tout tester
- Modifier uniquement les fichiers concernés par un chantier
- Après modification : tester les 5 onglets dans le navigateur

### Pour l'éditeur de cartes
- Fichier découpé en modules (HTML + JS séparés)
- Ne pas changer le format CARDS_SPECS (déjà validé et en production)

### Format CARDS_SPECS (figé, ne pas modifier)
```json
{
  "format": "CARDS_SPECS",
  "deck": {
    "name": "Nom du deck",
    "cards": [
      { "id": "card-1", "name": "Nom", "image": "card-1.png" }
    ]
  }
}
```
- Une image PNG par carte (660×1140, les deux faces dans l'image)
- Le JSON référence les PNG par nom de fichier
- Les PNG sont déposés à côté du JSON sur GitHub




