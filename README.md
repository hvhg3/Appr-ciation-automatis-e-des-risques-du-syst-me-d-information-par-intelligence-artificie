# Appréciation automatisée des risques du système d’information par intelligence artificielle

Un modèle d’apprentissage supervisé et sa plateforme d’exploitation, appliqués à la méthode EBIOS Risk Manager.  
Cas d’étude : **Banque Al Amane**.

## Guide pas à pas pour poser le projet

Ce dépôt sert de base pour construire et maîtriser le projet avec sa documentation.

### 1) Définir le cadre
- Sujet : appréciation automatisée des risques SI par IA.
- Méthode de référence : EBIOS Risk Manager.
- Cas d’étude : Banque Al Amane.
- Résultat attendu : une approche reproductible (méthode + données + modèle + interprétation).

### 2) Poser les objectifs
- Objectif principal : estimer/qualifier le niveau de risque à partir d’informations métier et techniques.
- Objectifs secondaires :
  - comparer plusieurs modèles supervisés,
  - expliquer les prédictions,
  - proposer une intégration opérationnelle.

### 3) Préparer les livrables documentaires
Créer et maintenir les documents suivants :
- `docs/01_cadrage.md` : contexte, périmètre, hypothèses.
- `docs/02_besoins_et_exigences.md` : besoins métier/fonctionnels.
- `docs/03_donnees.md` : sources, qualité, préparation, variables.
- `docs/04_methode_ebios_rm.md` : ateliers EBIOS RM et mapping vers données.
- `docs/05_modelisation_ia.md` : algorithmes, features, entraînement.
- `docs/06_evaluation.md` : métriques, validation, limites.
- `docs/07_deploiement_plateforme.md` : architecture cible et exploitation.
- `docs/08_plan_projet.md` : planning, jalons, risques projet.

### 4) Structurer le travail
Processus recommandé :
1. Cadrage (problème, périmètre, acteurs).
2. Collecte et qualification des données.
3. Conception de la grille de risque (alignée EBIOS RM).
4. Entraînement et comparaison des modèles.
5. Validation métier et technique.
6. Rédaction des conclusions et recommandations.

### 5) Définir les critères de réussite
- Traçabilité entre besoins, données, modèle et résultats.
- Métriques claires (ex. précision, rappel, F1, robustesse).
- Justification des choix méthodologiques.
- Documentation complète et compréhensible.

### 6) Routine de pilotage (chaque semaine)
- Mettre à jour `docs/08_plan_projet.md` (avancement, blocages, décisions).
- Mettre à jour les documents impactés (`docs/03`, `docs/05`, `docs/06`).
- Garder un historique court des changements dans les documents.

## Démarrage rapide
1. Créer le dossier `docs/`.
2. Ajouter les fichiers listés dans la section **3) Préparer les livrables documentaires**.
3. Commencer par `docs/01_cadrage.md` et `docs/08_plan_projet.md`.
4. Compléter ensuite les documents au fil des étapes du projet.
