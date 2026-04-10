# 🤸 Collège Pasteur — Gym Pasteur · Guide Firebase

## Configuration Firebase (eps-pasteur)

Le projet est connecté au projet Firebase **`eps-pasteur`**. La connexion est **entièrement automatique** au chargement du fichier — aucune saisie manuelle, aucun code à partager.

### Identifiants du projet

| Paramètre | Valeur |
|---|---|
| **Project ID** | `eps-pasteur` |
| **Database URL** | `https://eps-pasteur-default-rtdb.europe-west1.firebasedatabase.app` |
| **API Key** | `AIzaSyAJgRygktg-zkVSiTHgP2Co7AWb-E0I0Qw` |
| **Auth Domain** | `eps-pasteur.firebaseapp.com` |
| **Storage Bucket** | `eps-pasteur.firebasestorage.app` |
| **Messaging Sender ID** | `501398220240` |
| **App ID** | `1:501398220240:web:4a994b40016634e62b5dd5` |
| **Measurement ID** | `G-6ZGTYKDQJB` |

Ces identifiants sont intégrés directement dans le fichier HTML — les deux espaces (enseignant et élève) se connectent automatiquement au même projet sans aucune configuration.

---

## Flux de connexion — ce que fait chaque utilisateur

### Enseignant
1. Ouvrir `Gym_Pasteur_V15b_index.html` → page d'accueil
2. Cliquer **👨‍🏫 Espace Enseignant** → saisir le code PIN (défaut : `1234`)
3. Firebase se connecte automatiquement en arrière-plan
4. Les enchaînements des élèves arrivent en temps réel dans les onglets **📱 Élèves** et **⚖️ Jury**
5. Pour se déconnecter : bouton **⎋ Déconnexion** en haut à droite

### Élève
1. Ouvrir le même fichier HTML → page d'accueil
2. Cliquer **🤸 Espace Élève** → créer un compte (classe + prénom + mot de passe)
3. Composer son enchaînement → **📤 Envoyer au professeur**
4. Firebase transmet automatiquement → le prof voit arriver l'enchaînement sans rien faire
5. Quand le jury a noté → l'élève voit sa note dans **🏅 Voir ma note**

> **Aucun code de session à partager.** La connexion est directe et permanente via Firebase.

---

## Architecture de synchronisation

```
┌──────────────────────┐           Firebase RTDB             ┌───────────────────────┐
│    Espace Élève      │                                      │   Espace Enseignant   │
│                      │  → gym_sync/{classeNom}/{eleveId} → │  Onglet Jury / Élèves │
│  Envoie enchaînement │                                      │                       │
│  Reçoit note jury    │  ← gym_jury/{eleveId}             ← │  validateJuryScore()  │
└──────────────────────┘                                      └───────────────────────┘
         ↕                                                              ↕
   localStorage                                                   localStorage
   (copie locale)                                                  (copie locale)
```

Les deux espaces écrivent d'abord en `localStorage` (fonctionnement offline), puis synchronisent vers Firebase. Si Firebase est indisponible, tout fonctionne en local sur le même appareil.

### Nœuds Firebase utilisés

| Chemin Firebase | Sens | Usage |
|---|---|---|
| `gym_sync/{classeNom}/{eleveId}` | Élève → Enseignant | Enchaînement envoyé, affiché dans Jury et onglet Élèves |
| `gym_jury/{eleveId}` | Enseignant → Élève | Note validée par le jury, visible dans "Voir ma note" |

> L'ancien nœud `sessions/{code}` (code de session manuel) n'est plus utilisé.

---

## Règles de sécurité Firebase

Dans la console Firebase → **Build → Realtime Database → Règles** :

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> Ces règles sont adaptées à un usage pédagogique interne (réseau établissement). Pour un déploiement public, restreindre par authentification Firebase.

---

## Fonctionnement pas à pas

| Étape | Acteur | Action | Firebase |
|---|---|---|---|
| 1 | Enseignant | Ouvre le fichier | Firebase se connecte automatiquement |
| 2 | Élève | Crée son compte | Compte stocké en localStorage |
| 3 | Élève | Envoie son enchaînement | Écrit dans `gym_sync/` |
| 4 | Enseignant | Reçoit automatiquement | Listener `gym_sync/*` déclenché |
| 5 | Enseignant | Juge et valide la note | Écrit dans `gym_jury/` |
| 6 | Élève | Voit sa note | Listener `gym_jury/*` déclenché |

---

## Dépannage

### Firebase ne se connecte pas
- Vérifier que la Realtime Database est **activée** : console Firebase → Build → Realtime Database
- Vérifier que les règles sont bien à `true` (voir ci-dessus)
- Ouvrir la console navigateur (`F12`) et chercher les messages contenant `Firebase`
- Un message `✅ Firebase connecté automatiquement` doit apparaître dans l'onglet **📡 Connexion en temps réel**

### Un élève n'apparaît pas chez le prof
- Vérifier que l'élève a bien cliqué **📤 Envoyer au professeur** (et non juste validé)
- Cliquer **🔄 Actualiser** dans l'onglet Firebase de l'espace enseignant
- Vérifier que la classe de l'élève correspond exactement à une classe créée par le prof

### La note n'apparaît pas chez l'élève
- Vérifier que le prof a cliqué **✔ Valider la note** dans le panel jury
- L'élève doit recharger sa page **🏅 Voir ma note** ou attendre le listener Firebase

### Réinitialiser le code PIN enseignant (oublié)
Ouvrir la console navigateur (`F12`) et taper :
```js
resetTeacherPin('nouveaucode')
```

### Réinitialiser le mot de passe d'un élève (oublié)
```js
resetStudentPwd('5B', 'Marie', 'nouveaumdp')
```

---

## Modifier la configuration Firebase

Si tu changes de projet Firebase, chercher ces lignes dans le fichier HTML — elles apparaissent **deux fois** (une pour l'espace enseignant, une pour l'espace élève) :

```js
apiKey:      "AIzaSyAJgRygktg-zkVSiTHgP2Co7AWb-E0I0Qw",
databaseURL: "https://eps-pasteur-default-rtdb.europe-west1.firebasedatabase.app",
projectId:   "eps-pasteur",
```

Remplacer les trois valeurs par celles du nouveau projet Firebase.

---

## Synchronisation locale (même appareil, même navigateur)

Quand les deux espaces sont ouverts dans le **même navigateur** (deux onglets du même fichier HTML), la synchronisation passe aussi par `BroadcastChannel` — elle est alors instantanée et ne dépend pas de Firebase.
