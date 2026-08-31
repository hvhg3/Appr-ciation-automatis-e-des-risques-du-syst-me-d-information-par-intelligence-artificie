# PFA — Gestion des risques du système d'information avec module d'intelligence artificielle

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Étudiant** : Soufiane ATRAG  
**Formation** : ENSA Khouribga — Cycle Ingénieur MGSI, 1ʳᵉ année  
**Cas d'étude** : Banque Al Amane (cas fictif réaliste, secteur bancaire marocain)  
**Méthode métier** : EBIOS Risk Manager / ISO 27005  
**Date** : août 2026

---

## Licence

Ce projet est publié sous licence **MIT**. Voir le fichier [`LICENSE`](./LICENSE) pour les détails.

---
## Objet de cet envoi

Ce dossier présente l'état d'avancement du volet technique du projet, **avant rédaction
du rapport**. Il contient deux livrables :

1. le **modèle de classification** et sa démarche complète, sous forme de notebook ;
2. la **plateforme web** qui l'exploite en conditions d'usage.

Pour un premier regard sans aucune installation, le fichier `APERCU_INTERFACE.html`
placé à la racine s'ouvre d'un double-clic dans n'importe quel navigateur : il reproduit
l'interface complète de la plateforme, renseignée par une analyse réellement produite
par le modèle.

---

## 1_MODELE — Le cœur du travail

| Fichier | Contenu |
|---|---|
| `modele_classification_risques.ipynb` | **Notebook principal** — démarche complète en 9 parties, exécutée |
| `03_risques_original.csv` | Jeu de données issu de l'analyse EBIOS RM (63 risques) |
| `03_risques_augmente.csv` | Jeu équilibré après augmentation (78 risques, 26 par classe) |
| `03_risques_audite.csv` | Dataset enrichi des prédictions et de l'indicateur d'accord |
| `matrice_confusion_validation_croisee.png` | Figure destinée au chapitre 5 |
| `themes.png` | Répartition thématique des risques |

### Comment lire le notebook

Il se lit **de haut en bas**, sans exécution nécessaire : toutes les sorties sont
conservées. Chaque partie explique le raisonnement avant le code.

| Partie | Contenu |
|---|---|
| 1 → 3 | Données, préparation, vectorisation TF-IDF |
| 3 bis | Module de recommandation — règles métier issues de l'atelier 5 EBIOS RM |
| 4 → 6 | Entraînement Naive Bayes, évaluation, réglage du paramètre de lissage |
| 7 | Validation croisée — la mesure de performance retenue |
| 8 | Audit de cohérence — le modèle relit la matrice des risques |
| 9 | Exports et synthèse |

Pour l'exécuter : `pip install pandas scikit-learn matplotlib joblib`,
puis ouvrir le notebook dans Jupyter ou VS Code. Le CSV doit rester dans le même dossier.

### Résultats principaux

| Indicateur | Valeur |
|---|---|
| Corpus | 78 risques, 26 par classe |
| Vocabulaire TF-IDF | 463 termes, dont **67 % vus une seule fois** |
| Algorithme | Naive Bayes multinomial, `alpha = 0,3` |
| **Accuracy (validation croisée 5 plis)** | **61,8 %** — écart-type 0,119 |
| Référence aléatoire | 33 % |
| Rappel par classe | faible 0,46 · moyen 0,77 · élevé 0,62 |
| Divergences relevées par l'audit | 6 sur 78 (au seuil de confiance 0,55) |

### Trois points soumis à votre appréciation

**Le chiffre de performance retenu est 61,8 %, non 75 %.** Le découpage 80/20 donnait
75 %, mais dix tirages différents produisent des scores allant de 37,5 % à 75 % : cette
mesure n'était pas exploitable sur un corpus de cette taille. La validation croisée
stratifiée a donc été substituée (partie 7, bloc 28 pour la démonstration).

**L'augmentation de données a un coût, documenté.** Le jeu d'origine ne comptait que
11 risques `faible`, concentrés sur 4 actifs sur 8. Quinze descriptions ont été rédigées
pour rétablir l'équilibre. L'une d'elles a introduit un biais mesurable : le mot
« transaction », employé dans un contexte de faible criticité, oriente désormais le
modèle dans cette direction. Ce défaut est identifié en partie 8.

**L'audit a détecté une incohérence de cotation.** Le risque *« Des données de test
contenant des informations réelles de clients sont oubliées sur un serveur non sécurisé »*
a été coté `moyen` ; le modèle prédit `élevé` avec une confiance de 0,59. S'agissant
d'une exposition de données personnelles — loi 09-08, compétence CNDP — la prédiction
du modèle paraît fondée. La cotation initiale mérite révision.

---

## 2_PLATEFORME — L'application web

Interface professionnelle exploitant le modèle en boîte noire : elle lui transmet les
descriptions et affiche les résultats, sans intervenir sur son fonctionnement interne.

### Aperçu immédiat, sans installation

`APERCU_INTERFACE.html`, à la racine de cet envoi : double-clic, le navigateur s'ouvre
sur l'interface complète. La navigation, les filtres, la matrice de criticité, le détail
des risques et le rapport imprimable y sont fonctionnels. Les résultats affichés
proviennent d'une analyse réelle du modèle, figée dans la page — le moteur Python, lui,
n'y est pas embarqué. Une pastille le signale dans le bandeau du tableau de bord.

### Lancement de l'application complète

Sur macOS : **double-cliquer `LANCER.command`**. La première exécution installe
l'environnement Python (1 à 3 minutes), puis ouvre le navigateur automatiquement.

Sinon, depuis le dossier `interface_risques` :

```bash
pip install -r requirements.txt
uvicorn app:app --reload
```
puis ouvrir http://127.0.0.1:8000

### Parcours

Fiche de l'organisation auditée → choix du référentiel parmi onze (EBIOS RM,
ISO 27001/27005/31000, NIST CSF/RMF, COBIT, CIS Controls, PCI DSS, RGPD) →
questionnaire adapté au référentiel → saisie ou import des risques → analyse →
résultats graphiques → export Excel et PDF → recueil du retour d'expérience.

### Architecture

Trois couches étanches : `static/index.html` pour la présentation, `app.py` pour
l'orchestration et la validation, `moteur.py` pour l'accès au modèle. Le détail des
choix techniques figure dans `interface_risques/LANCEMENT.md`.

La présentation est isolée dans des feuilles de style autonomes : `static/style.css`
porte le système de design « Cyber-Analyse » — fond anthracite, accent unique orange
sécurité réservé à l'action, code couleur rouge / jaune / vert réservé à la donnée
produite par le modèle, monospace pour toute donnée brute ; `static/circuit.css` et
`static/circuit.js` ajoutent le filigrane de circuit imprimé, son flux de données et
le sceau en trame de points dont la couleur d'état suit le niveau d'exposition.
La note d'intégration se trouve dans `interface_risques/static/REFONTE_UI.md`.

---

## Ce qui reste à produire

La rédaction du rapport. Le matériau est disponible : le notebook documente chaque
décision et sa justification, les figures sont exportées, et les résultats chiffrés
sont consolidés.

---

**Soufiane ATRAG**
atrag.soufiane@gmail.com · +212 7 01 31 31 92
https://www.linkedin.com/in/atrag-soufiane-569b6b203/
