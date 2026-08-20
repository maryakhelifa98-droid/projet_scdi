# CLAUDE.md

Ce fichier donne à Claude Code le contexte nécessaire pour travailler sur ce projet.

## Vue d'ensemble du projet

Site vitrine présentant une **étude de data science sur la performance des joueurs de
la Coupe du Monde FIFA 2026**, réalisée en Python (pandas, seaborn, statsmodels,
scikit-learn) à partir d'un dataset Kaggle synthétique.

L'étude répond à la problématique : *"Les profils de performance des joueurs de la
Coupe du Monde 2026 varient-ils selon le poste occupé, la confédération d'origine et
la valeur marchande ?"*

Le site doit vulgariser et mettre en scène les résultats de 3 notebooks Jupyter déjà
réalisés (fournis en pièce jointe / dans `/notebooks` du repo) :

1. **`01_nettoyage.ipynb`** — Nettoyage et contrôle qualité des données
2. **`02_analyse_descriptive.ipynb`** — Analyse descriptive et tests statistiques
3. **`03_classification_poste.ipynb`** — Classification du poste par régression logistique

⚠️ **Ce site est un livrable de démonstration/pédagogique**, pas un produit commercial.
Le ton doit rester clair, honnête sur les limites des données, et accessible à un public
non-spécialiste (jury, professeur, recruteur) tout en montrant la rigueur méthodologique.

## Stack technique

- **Framework** : [Astro](https://astro.build) (site statique / MPA, `.astro` components)
- **Styling** : à définir avec Claude Code au démarrage — recommandé : Tailwind CSS
  (`@astrojs/tailwind`) pour la rapidité d'itération
- **Graphiques** : les visualisations peuvent être ré-exportées en images (PNG/SVG)
  depuis les notebooks (`plt.savefig(...)`, mis dans `public/img/charts/`), ou
  reconstruites en JS léger (Chart.js / une petite lib SVG) si on veut de l'interactivité.
  Par défaut : privilégier des images statiques exportées des notebooks pour rester
  fidèle aux résultats réels, sauf demande explicite d'interactivité.
- **Déploiement cible** : à préciser (Vercel / Netlify / GitHub Pages) — poser la
  question si non précisé avant de configurer `astro.config.mjs`.
- **Langue du site** : français

## Structure du contenu (pages / onglets)

Le site doit organiser l'histoire en plusieurs sections claires, dans cet ordre logique :

1. **Accueil (`/`)** — Titre du projet, accroche, contexte du dataset (FIFA World Cup
   2026 Player Performance, Kaggle), problématique, aperçu visuel (logo/héro).
2. **Étude / Résultats (`/etude`)** — Le cœur du site, orienté grand public :
   - Distribution des statistiques individuelles (buts, minutes jouées, valeur
     marchande…), avec mention de l'asymétrie des distributions
   - Profils par poste (Gardien / Défenseur / Milieu / Attaquant) : contribution
     offensive vs défensive, test de Kruskal-Wallis significatif (p ≈ 1,5×10⁻⁷⁹)
   - Comparaison par confédération : **aucune différence significative** détectée
     (Kruskal-Wallis, p = 0,95) malgré des effectifs déséquilibrés (UEFA 37 % vs
     CONCACAF 12 %)
   - Valeur marchande vs performance réelle : corrélation positive mais modérée
     (Pearson r = 0,36 ; Spearman ρ = 0,36 ; R² ≈ 13 % en univarié, 58,8 % dans le
     modèle multiple avec poste + âge)
   - Cas particulier des gardiens (save_percentage, clean sheets…)
3. **Classification (`/classification`)** — Présentation du modèle de régression
   logistique multinomiale qui prédit le poste à partir de 12 statistiques de jeu :
   - Accuracy de 100 % (avec et sans les gardiens) → **présenté comme un signal
     d'alerte méthodologique**, pas une performance à célébrer
   - Matrice de confusion parfaitement diagonale
   - Coefficients interprétables par poste (tacles/interceptions → défenseurs ;
     buts/tirs → attaquants)
4. **Méthodologie / Documentation (`/methodologie`)** — Partie technique et honnête :
   - Nettoyage des données (1 248 joueurs, 75 colonnes, aucune valeur manquante,
     aucun doublon)
   - **Limite majeure à mettre en avant partout où c'est pertinent** : le dataset est
     **synthétique/simulé**, pas les vraies données de la compétition (calendrier
     irréaliste : 1 050 matchs recensés contre 104 dans un vrai Mondial à 48 équipes ;
     save_percentage moyen ≈ 23 % très en dessous des standards réels 65-75 %).
     Les résultats doivent être présentés comme une **illustration méthodologique**,
     pas comme une analyse de la vraie Coupe du Monde 2026.
   - Table de correspondance nationalité → confédération (choix méthodologiques,
     ex. Australie rattachée à l'AFC)
5. **À propos (`/a-propos`)** — Optionnel : auteur, source du dataset (lien Kaggle),
   outils utilisés, lien vers le code/notebooks (GitHub).

## Principes de contenu

- **Toujours nuancer** : chaque résultat statistique fort (p-value très faible, accuracy
  100 %) doit être accompagné de son interprétation critique et de ses limites, telles
  que formulées dans les notebooks (ex : "un score parfait est un signal d'alerte, pas
  une victoire").
- **Vulgariser sans trahir** : expliquer Kruskal-Wallis, p-value, R², corrélation en
  langage clair (analogies, encadrés "Comment lire ce chiffre"), sans jargon non expliqué.
- **Distinguer** clairement le contenu "grand public" (page Étude/Classification) du
  contenu "technique" (page Méthodologie), comme le prévoit déjà la synthèse du
  notebook 2.
- Ne jamais présenter les résultats comme s'ils décrivaient la vraie Coupe du Monde 2026 :
  toujours rappeler la nature synthétique du dataset dès que des chiffres bruts sont cités.

## Conventions de code pour Claude Code

- Composants Astro dans `src/components/`, pages dans `src/pages/`, layouts dans
  `src/layouts/`.
- Un composant réutilisable pour les cartes de statistiques clés (`StatCard.astro`)
  et un pour les encadrés d'interprétation ("Comment lire ce résultat").
- Images de graphiques dans `public/img/charts/`, nommées par section
  (`ex: distribution-buts.png`, `confusion-matrix.png`).
- Logo et assets de marque dans `public/img/brand/`.
- Préférer des composants simples et sémantiques (HTML accessible : `<figure>`,
  `<figcaption>` pour les graphiques, titres hiérarchisés correctement).
- Toujours tester `npm run build` avant de considérer une tâche terminée.

## Ce que Claude Code NE doit PAS faire sans demander

- Ne pas inventer de résultats statistiques non présents dans les notebooks fournis.
- Ne pas changer la structure des pages ci-dessus sans confirmation si le projet a déjà
  commencé.
- Ne pas choisir de plateforme de déploiement / nom de domaine sans validation.

## À faire en tout début de session

1. Vérifier si un projet Astro existe déjà (`package.json`, `astro.config.mjs`) ; sinon
   lancer `npm create astro@latest`.
2. Demander la palette de couleurs/typographie souhaitée si non précisée (voir logo
   fourni comme point de départ possible pour l'identité visuelle : bleu foncé,
   accent rouge/vert type terrain de foot).
3. Récupérer les exports de données (`players_clean.csv`, `matches_clean.csv`) et les
   figures des notebooks à réutiliser sur le site.
