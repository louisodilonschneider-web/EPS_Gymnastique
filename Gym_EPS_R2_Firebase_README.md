# 🤸 Collège Pasteur — Gym Pasteur · Guide Firebase

## Configuration Firebase (eps-pasteur)

Le projet est connecté au projet Firebase **`eps-pasteur`**. La connexion est **automatique** — aucune saisie manuelle requise à chaque utilisation.

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

---

## Architecture de synchronisation

```
┌─────────────────────┐         Firebase RTDB          ┌──────────────────────┐
│   Espace Élève      │  ──── gym_sync/{classe}/{id} ─→ │  Espace Enseignant   │
│  (eleve.html ou     │                                  │  (index.html)        │
│  espace élève dans  │  ←── gym_jury/{eleveId}  ──────  │  validateJuryScore() │
│  l'app)             │                                  │                      │
└─────────────────────┘                                  └──────────────────────┘
```

### Nœuds Firebase utilisés

| Chemin Firebase | Usage |
|---|---|
| `gym_sync/{classeNom}/{eleveId}` | Enchaînement envoyé par l'élève → lu par le prof |
| `gym_jury/{eleveId}` | Note validée par le prof → lue par l'élève |
| `sessions/{code}` | Code de session temporaire (ancien système) |

---

## Règles de sécurité Firebase

Dans la console Firebase → Realtime Database → Règles, utilise :

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> **Note** : Ces règles sont permissives (usage pédagogique interne). Pour un déploiement plus large, restreindre en authentification.

---

## Fonctionnement automatique

1. **À l'ouverture du fichier HTML** : Firebase se connecte automatiquement avec les identifiants ci-dessus.
2. **Quand un élève envoie son enchaînement** : il est écrit dans `localStorage` ET poussé vers `gym_sync/` sur Firebase.
3. **Quand le prof reçoit** : l'onglet Firebase écoute `gym_sync/*` en temps réel et met à jour les fiches élèves automatiquement.
4. **Quand le jury note** : le résultat est poussé vers `gym_jury/{eleveId}` et l'élève le reçoit en temps réel.

---

## Dépannage

### Firebase ne se connecte pas
- Vérifier que la base de données Firebase est **activée** sur la console → Build → Realtime Database
- Vérifier que les règles autorisent la lecture/écriture (voir ci-dessus)
- Ouvrir la console navigateur (F12) → chercher les erreurs `Firebase`

### Changer la configuration Firebase
Si tu changes de projet Firebase, ouvrir le fichier HTML et chercher les trois lignes :
```js
apiKey:      "AIzaSyAJgRygktg-zkVSiTHgP2Co7AWb-E0I0Qw",
databaseURL: "https://eps-pasteur-default-rtdb.europe-west1.firebasedatabase.app",
projectId:   "eps-pasteur",
```
Ces lignes apparaissent **deux fois** dans le fichier (une pour l'espace enseignant, une pour l'espace élève).

---

## Synchronisation locale (même appareil)

Si les deux espaces sont ouverts dans le **même navigateur** (deux onglets), la synchronisation passe aussi par `BroadcastChannel` — instantanée sans Firebase.

