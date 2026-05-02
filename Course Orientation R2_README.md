# 🧭 Course d'Orientation EPS

Application web complète pour gérer des courses d'orientation en milieu scolaire.  
Un seul fichier HTML — aucune installation côté serveur.

---

## 📋 Fonctionnalités

### Interface Élève (sans code)
| Onglet | Description |
|--------|-------------|
| 🏅 **Classement** | Classement live mis à jour en temps réel, chrono visible |
| 📊 **Mes Stats** | Historique personnel par séance, graphe de progression |

### Interface Enseignant (code : `1234`)
| Onglet | Description |
|--------|-------------|
| 🏫 **Classes** | Création/chargement de classes, ajout d'élèves, gestion présences/absences/blessés |
| ⚙️ **Leçon** | Modalités : type de parcours (circuit / étoile / papillon), format (solo/binôme/trinôme), nb de balises, durée |
| 👥 **Groupes** | Tablette prêtée aux élèves pour former leurs binômes/trinômes |
| 📍 **Déclaration** | Interface en 3 étapes : l'élève choisit son nom → sélectionne ses balises → l'enseignant valide |
| 📡 **Suivi** | Chronomètre maître, vue globale de chaque groupe avec LED par balise, code couleur de performance |
| ✅ **Validation** | File d'attente des déclarations en attente |

### Système de performance (onglet Suivi)
- 🟢 **Vert** — En avance sur le ratio balises/temps attendu
- 🟠 **Orange** — Dans la moyenne
- 🔴 **Rouge** — En difficulté (à surveiller)
- ⚫ **Gris** — Pas encore commencé

---

## 🚀 Mise en place Firebase (étape par étape)

> Firebase permet la **synchronisation temps réel** : l'enseignant valide sur sa tablette, les élèves voient le classement se mettre à jour instantanément sur leur téléphone.

### Étape 1 — Créer un projet Firebase

1. Va sur [https://console.firebase.google.com](https://console.firebase.google.com)
2. Clique sur **"Ajouter un projet"**
3. Donne un nom : ex. `orientation-eps-monecole`
4. **Désactive Google Analytics** (pas nécessaire) → Clique sur "Créer le projet"
5. Attends 30 secondes que Firebase initialise

### Étape 2 — Activer la Realtime Database

1. Dans le menu de gauche : **Build → Realtime Database**
2. Clique sur **"Créer une base de données"**
3. Choisis la région : **europe-west1 (Belgium)** ✅
4. Mode de démarrage : choisis **"Commencer en mode test"** *(on sécurisera après)*
5. Clique sur **"Activer"**

### Étape 3 — Configuration Firebase ✅ Déjà intégrée dans index.html

La configuration du projet **course-orientation-f89c3** est déjà présente dans `index.html` :

```js
const firebaseConfig = {
  apiKey: "AIzaSyAERTdpxZrUYplMl0TaQryAhEKbfS3_jfo",
  authDomain: "course-orientation-f89c3.firebaseapp.com",
  databaseURL: "https://course-orientation-f89c3-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "course-orientation-f89c3",
  storageBucket: "course-orientation-f89c3.firebasestorage.app",
  messagingSenderId: "119825222923",
  appId: "1:119825222923:web:d1d3884d45233caf20260e",
  measurementId: "G-KK7HFZSQ1V"
};
```

> ⚠️ La seule ligne à vérifier après création de la Realtime Database est **`databaseURL`** : ouvre la Realtime Database dans la console Firebase, l'URL exacte est affichée en haut de la page. Si elle diffère, corrige-la dans `index.html` ligne ~22.

### Étape 4 — Vérifier la databaseURL

1. Va dans la console Firebase → menu gauche → **Realtime Database**
2. L'URL affichée en haut ressemble à :  
   `https://course-orientation-f89c3-default-rtdb.europe-west1.firebasedatabase.app`
3. Si elle est identique → **rien à faire**, le fichier est prêt ✅  
4. Si elle diffère → ouvre `index.html`, trouve la ligne `databaseURL:` et corrige-la

### Étape 5 — Sécuriser la base de données

Dans la console Firebase → **Realtime Database → Règles**, colle ceci :

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ Ces règles permettent à n'importe qui de lire/écrire. C'est acceptable pour un usage interne en réseau scolaire. Pour un usage plus large, contacte un développeur pour ajouter une authentification.

Clique sur **"Publier"**.

### Étape 6 — Tester

1. Ouvre `index.html` dans un navigateur (Chrome recommandé)
2. Ouvre le même fichier sur ton téléphone (en le partageant sur un serveur local ou via GitHub Pages)
3. Active le mode enseignant (code : `1234`) et configure une leçon test
4. Depuis un autre appareil, vérifie que le classement se met à jour

---

## 📡 Comment les élèves accèdent au classement

### Option A — Réseau Wi-Fi local (recommandée en gymnase)
1. Héberge le fichier avec un petit serveur local sur ta tablette :
   - **Windows** : installe [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) dans VS Code
   - **Mac** : dans le terminal, navigue vers le dossier et tape `python3 -m http.server 8080`
2. Ton adresse locale s'affiche (ex: `192.168.1.42:8080`)
3. Génère un QR code avec [qr-code-generator.com](https://qr-code-generator.com) pointant vers cette adresse
4. Les élèves scannent le QR code → accèdent directement au classement

### Option B — GitHub Pages (accessible partout avec internet)
1. Crée un compte [GitHub](https://github.com) gratuit
2. Crée un dépôt public et uploade `index.html`
3. Active GitHub Pages dans les paramètres du dépôt
4. Tu obtiens une URL publique : `https://tonpseudo.github.io/orientation-eps/`
5. Partage cette URL ou génère un QR code

### Option C — Google Drive / OneDrive
Moins idéale pour le temps réel, mais possible pour le classement différé.

---

## 🎮 Guide d'utilisation rapide

### Avant la séance
1. 🏫 **Classes** → créer ou charger ta classe, marquer absents/blessés
2. ⚙️ **Leçon** → configurer le parcours (ex: Papillon, 20 balises, 20 min, Binôme)
3. 👥 **Groupes** → prêter la tablette à chaque binôme pour se former
4. ⚙️ **Leçon** → cliquer **🚀 Lancer la course**

### Pendant la séance
1. 📡 **Suivi** → démarrer le chronomètre ▶
2. Quand un élève revient au point de repère :
   - 📍 **Déclaration** → il sélectionne son nom, puis ses balises
   - L'enseignant valide ✅ ou refuse ❌
3. Les couleurs dans **Suivi** se mettent à jour automatiquement
4. Les élèves voient leur classement sur **leur téléphone** (onglet Classement)

### Après la séance
- Les statistiques sont automatiquement sauvegardées dans Firebase
- Les élèves peuvent consulter leur progression dans **📊 Mes Stats**

---

## ⚙️ Personnalisation

### Changer le code PIN enseignant
Dans `index.html`, cherche :
```js
pin: '1234',
```
Remplace `1234` par ton code.

### Adapter le score
Le score est calculé ainsi :
```
Score = (nombre de balises × 100) − temps en secondes
```
Tu peux modifier la fonction `computeScore()` dans le JS.

---

## 🛠️ Dépannage

| Problème | Solution |
|----------|----------|
| Firebase non configuré | Vérifier que la config Firebase est bien collée dans index.html |
| Le classement ne se met pas à jour | Vérifier la connexion internet des élèves |
| Erreur "Permission denied" | Vérifier les règles de sécurité Firebase (étape 5) |
| Les données ne s'effacent pas entre séances | Utiliser "🚀 Lancer la course" remet tout à zéro |
| Le chrono ne se synchronise pas | Le chrono se sync toutes les 5 secondes, normal |

---

## 📱 Compatibilité

- ✅ Chrome (recommandé)
- ✅ Firefox
- ✅ Safari (iOS)
- ✅ Chrome Android
- ⚠️ Internet Explorer : non supporté

---

## 📄 Fichiers

```
orientation-eps/
└── index.html    ← L'application complète (tout-en-un)
└── README.md     ← Ce guide
```

---

*Développé pour l'EPS — Course d'Orientation scolaire*
