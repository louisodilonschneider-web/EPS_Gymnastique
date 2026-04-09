# 💃 Collège Pasteur — Gymnastique Artistique

Application web **mono-fichier HTML** dédiée à la gestion des cours de gymnastique au sol au Collège Pasteur. Elle couvre l'intégralité du cycle : composition des enchaînements, suivi par classe et par leçon, jugement et résultats, synchronisation entre appareils.

> **Aucune installation requise.** Ouvrir le fichier `Gym_Pasteur_V15b_index.html` dans un navigateur suffit. Toutes les données sont conservées localement dans le navigateur (`localStorage`).

---

## Sommaire

1. [Vue d'ensemble](#vue-densemble)
2. [Onglet Classe](#-onglet-classe)
3. [Onglet Enchaînement](#-onglet-enchaînement)
4. [Onglet Jury](#-onglet-jury)
5. [Onglet Résultats](#-onglet-résultats)
6. [Onglet Synchronisation](#-onglet-synchronisation)
7. [Onglet Figures en vidéo](#-onglet-figures-en-vidéo)
8. [Barème et notation](#barème-et-notation)
9. [Figures disponibles](#figures-disponibles)
10. [Données et confidentialité](#données-et-confidentialité)

---

## Vue d'ensemble

| Onglet | Rôle |
|---|---|
| 🏫 Classe | Gérer classes, leçons, élèves · Suivre les absences/blessures |
| ✏️ Enchaînement | Composer et valider un enchaînement par élève |
| ⚖️ Jury | Juger un enchaînement et saisir les déductions |
| 🏆 Résultats | Consulter les classements par type de séance |
| 🔄 Synchronisation | Partager les données entre plusieurs appareils |
| 🎬 Figures en vidéo | Accéder aux vidéos de référence de l'Académie de Créteil |

---

## 🏫 Onglet Classe

Point d'entrée principal de l'application. Tout s'organise à partir d'ici.

### Créer une classe

1. Saisir le nom de la classe (ex. `5B`, `4A`) dans le champ prévu
2. Cliquer sur **+ Ajouter** (ou appuyer sur Entrée)
3. La classe apparaît sous forme de pill cliquable

Plusieurs classes peuvent coexister. La classe active est mise en évidence en orange.

### Créer une leçon

Chaque classe peut contenir plusieurs leçons. Chaque leçon a un **type** qui détermine dans quelle colonne ses résultats apparaîtront :

| Type | Icône | Usage |
|---|---|---|
| Entraînement | 🏋️ | Séances de travail (multiples) |
| Évaluation 1 | 📋¹ | Première évaluation notée |
| Évaluation 2 | 📋² | Deuxième évaluation notée |

### Gérer les élèves

#### Ajouter manuellement
Saisir « Prénom Nom » dans le champ et cliquer **+ Élève**.

#### Importer depuis un fichier 📂
Le bouton **📂 Importer** permet de charger la liste depuis :

- **CSV / TXT** — un nom par ligne (séparateurs acceptés : retour à la ligne, virgule, point-virgule)
- **PDF** — fonctionne si le PDF est généré numériquement (pas un scan)

Un aperçu s'affiche avant l'import avec :
- Cases à cocher pour sélectionner les élèves à importer
- Badge **"Déjà dans la classe"** pour signaler les doublons (exclus automatiquement)
- Bouton **Tout cocher / décocher**
- Capitalisation automatique (`DUPONT MARIE` → `Dupont Marie`)

#### Statuts de présence

Sur chaque carte élève, deux boutons permettent de basculer le statut :

| Bouton | Statut | Couleur |
|---|---|---|
| ❌ Absent | Absent | Rouge |
| 🩹 Blessé | Blessé | Jaune |
| *(re-cliquer)* | Présent | Bleu |

Le bouton **↺ Présence** remet tous les élèves à « Présent » en un clic.

Une barre de résumé affiche en temps réel le décompte ✅ présents · ❌ absents · 🩹 blessés.

### Fiche élève (vue globale)

Quand **aucune leçon n'est sélectionnée**, chaque carte élève affiche :

- ✔ **Dernier enchaînement validé** (toutes leçons confondues) avec sa Note D et sa date
- 🏋️ **Nombre de passages en entraînement** et la **Note D moyenne**

Quand **une leçon est sélectionnée**, la carte affiche le nombre de passages et la Note D du dernier passage pour cette leçon précise.

### Accéder à l'enchaînement d'un élève

Cliquer sur le **prénom souligné** de l'élève ouvre l'onglet Enchaînement pré-rempli avec son nom, son dernier enchaînement validé chargé, et un fil d'Ariane indiquant la classe, la leçon et le nom.

---

## ✏️ Onglet Enchaînement

Cet onglet est accessible directement depuis la fiche élève ou via la barre de navigation.

### Fil d'Ariane

En haut de l'onglet, le fil d'Ariane indique : `Classe › Leçon › Nom de l'élève · X passage(s)`.

Le bouton **← Classe** permet de revenir à la liste sans perdre les données.

### Fiche des passages

Sous le fil d'Ariane, la **fiche élève** affiche l'historique de tous les passages déjà validés pour la leçon en cours, du plus récent au plus ancien. Chaque passage indique :

- Son numéro et l'heure de validation
- La **Note D** obtenue
- La liste des figures sélectionnées avec leur catégorie (A/B/C/D)

Le dernier passage est mis en évidence (bordure orange + badge « Dernier »).

### Composer l'enchaînement

Les figures sont organisées en quatre catégories de difficulté croissante. Cliquer sur une figure l'ajoute à l'enchaînement (ou la retire si elle y est déjà). Les figures sélectionnées sont mises en évidence.

L'aperçu en bas de la liste affiche en temps réel :
- **Diff.** (X/8 fig.) — somme des valeurs des 8 meilleures figures
- **Exig.** (X/4) — nombre d'exigences remplies
- **Total Note D** /10

L'enchaînement peut être réordonné par **glisser-déposer** (souris ou tactile).

### Valider un passage

Le bouton **✔ Valider ce passage** enregistre l'enchaînement sur l'élève pour la leçon sélectionnée. L'enchaînement est ensuite vidé automatiquement pour permettre un **passage immédiat**.

Le bouton **+ Nouveau passage** permet de vider manuellement sans valider.

Chaque passage validé est conservé dans l'**historique complet** de l'élève, par leçon.

### Télécharger la fiche

Le bouton **⬇ Télécharger ma fiche** génère une fiche PDF imprimable reprenant : le nom, l'agrès, les exigences remplies, le tableau des figures et la Note D.

---

## ⚖️ Onglet Jury

### Liste des gymnastes à juger

Tous les élèves ayant au moins un enchaînement validé apparaissent dans la liste, avec leur classe, leur leçon et le type de séance (🏋️ Entraînement · 📋¹ Éval.1 · 📋² Éval.2).

Cliquer sur **Juger →** ouvre le panneau de notation.

### Panneau de jugement

En haut du panneau :

- **Nom du juge** — champ libre (nom d'un élève, de l'enseignant…)
- **Badge type de séance** — rappelle le contexte (entraînement ou évaluation)

#### Fautes spécifiques Sol

| Faute | Déduction |
|---|---|
| Figure oubliée | −1,00 pt |
| Latence entre deux figures | −0,10 pt |
| Latence fréquente | −0,30 pt |
| Latence permanente | −0,50 pt |
| Enchaînement non connu | −1,00 pt |

#### Notation par figure

Pour chaque figure de l'enchaînement, quatre boutons permettent de déduire :

| Bouton | Valeur |
|---|---|
| Légère | −0,10 pt |
| Moyenne | −0,30 pt |
| Grosse | −0,50 pt |
| Chute | −1,00 pt |

Les boutons **↺ Annuler** et **✕ Reset** permettent de corriger une saisie.

#### Calcul Note E

Le panneau affiche en temps réel :
- Note E brute (10,00)
- Total des pénalités
- Note E finale
- **Note finale = Note D + Note E − pénalités Jury D**

Un champ **Pénalités Jury D additionnelles** et un champ **Commentaire** sont disponibles.

#### Valider la note

Le bouton **✔ Valider la note** enregistre le résultat sur l'élève dans la leçon concernée. Le résultat est immédiatement visible dans l'onglet Résultats.

---

## 🏆 Onglet Résultats

Les résultats sont séparés en trois catégories accessibles par des onglets :

| Onglet | Contenu |
|---|---|
| 🏋️ Entraînements | Tous les passages jugés lors des séances d'entraînement |
| 📋 Évaluation 1 | Résultats de la première évaluation |
| 📋 Évaluation 2 | Résultats de la deuxième évaluation |

Pour chaque catégorie, le tableau affiche : classement, nom de l'élève, classe · leçon, Note D, Note E, pénalités, **Note finale**, et nom du juge.

Les boutons **📄 Export PDF** et **🗑 Réinitialiser** sont disponibles en haut de l'onglet.

---

## 🔄 Onglet Synchronisation

Permet de partager les données entre plusieurs appareils (tablettes, téléphones) via une session commune.

### Session partagée

1. Un appareil génère un **code de session** (6 caractères) avec **⚡ Générer**
2. Les autres appareils saisissent ce code et cliquent **🔗 Rejoindre**
3. Les appareils connectés apparaissent dans la liste des pairs

Les boutons **⬆ Envoyer mes données** et **⬇ Recevoir les données** permettent la synchronisation manuelle.

### Import / Export de données

| Format | Usage |
|---|---|
| JSON — Complet | Export/import de tous les gymnastes |
| JSON — Résultats | Export des gymnastes jugés uniquement |
| PDF — Classement | Rapport complet imprimable |
| JSON — Sauvegarde session | Snapshot avec métadonnées pour archivage |

L'import est **intelligent** : il détecte les doublons et propose de fusionner, remplacer ou conserver en cas de conflit.

---

## 🎬 Onglet Figures en vidéo

Lien direct vers les vidéos de référence de la Gymnastique Artistique UNSS sur le site de l'**Académie de Créteil**. Idéal pour que les élèves visualisent les figures avant de composer leur enchaînement.

---

## Barème et notation

### Note D — Difficulté

| Élément | Valeur |
|---|---|
| Figure catégorie A | 0,40 pt |
| Figure catégorie B | 0,60 pt |
| Figure catégorie C | 0,80 pt |
| Figure catégorie D | 1,00 pt |

- Seules les **8 meilleures figures** sont retenues (maximum 8 points)
- **Aucune pénalité** si l'enchaînement comporte moins de 8 figures

### Exigences de composition (+2 pts)

0,50 pt est accordé pour chaque exigence remplie (max 4) :

| Code | Exigence |
|---|---|
| ATR | Appui Tendu Renversé |
| SM | Souplesse / Maintien |
| LG | Liaison Gymnique (2 éléments LG consécutifs et différents) |
| AC | 2 éléments Acrobatiques dans des sens différents (AV / AR / Latéral) |

**Note D totale = Difficulté (max 8) + Exigences (max 2) = max 10 pts**

### Note E — Exécution

Départ à **10,00 pts**, déductions appliquées par le jury selon les fautes observées.

**Note finale = Note D + Note E**

---

## Figures disponibles

### Catégorie A — Difficulté 0,40 pt

| # | Figure | Exigence |
|---|---|---|
| A1 | Poirier | ATR |
| A2 | Roue mains surélevées | ATR |
| A3 | Roulade AV sur triangle | AC |
| A4 | Roulade AR sur triangle | AC |
| A5 | Saut vertical | LG |
| A6 | ½ tour ½ pointes | LG |
| A7 | Pas chassé | LG |
| A8 | Grand battement | SM |
| A9 | Onde dorsale | SM |
| A10 | Salto AV avec triangle | AC |

### Catégorie B — Difficulté 0,60 pt

| # | Figure | Exigence |
|---|---|---|
| B1 | ATR ventre face au mur | ATR |
| B2 | Roue avec obstacles | ATR |
| B3 | Roulade AV groupée | AC |
| B4 | Roulade AR écartée | AC |
| B5 | Saut de chat | LG |
| B6 | Saut vertical ½ tour | LG |
| B7 | Pont tenu 2s | SM |
| B8 | Salto AV appui sur bloc | AC |

### Catégorie C — Difficulté 0,80 pt

| # | Figure | Exigence |
|---|---|---|
| C1 | ATR contre le mur | ATR |
| C2 | Roue | ATR |
| C3 | 2 roulades AV groupées | AC |
| C4 | Roulade AR groupée | AC |
| C5 | Saut vertical tour complet | LG |
| C6 | Saut groupé | LG |
| C7 | Chandelle tenu 2s | SM |
| C8 | Salto AV effleurant le bloc | AC |

### Catégorie D — Difficulté 1,00 pt

| # | Figure | Exigence |
|---|---|---|
| D1 | ATR roulade ou ½ tour | ATR |
| D2 | Rondade | ATR |
| D3 | 2 roulades AV élevées (×2) | AC |
| D4 | 2 roulades AR | AC |
| D5 | Tour et ½ | LG |
| D6 | Saut de chat ½ tour | LG |
| D7 | Planche faciale | SM |
| D8 | Salto AV groupé | AC |

---

## Données et confidentialité

- Toutes les données (classes, élèves, enchaînements, notes) sont stockées **uniquement dans le navigateur** via `localStorage`
- **Aucune donnée n'est envoyée vers un serveur externe**
- La fonction de synchronisation utilise un réseau pair-à-pair (WebRTC/PeerJS) : les données transitent directement entre appareils sans serveur intermédiaire
- Vider le cache du navigateur efface toutes les données — utiliser l'export JSON régulièrement pour sauvegarder
- L'application fonctionne **hors connexion** une fois le fichier chargé (sauf la synchronisation et les vidéos de référence)

---

*Application développée pour le Collège Pasteur · Cycle Gymnastic Artistique Sol 2024–2028*
