# frahlweb

Page d'accueil / navigateur web du Frahl (RP Minecraft).

## Déploiement sur GitHub Pages

1. Crée un dépôt (ex: `frahlweb`) et pousse `index.html` à la racine.
2. Dans les paramètres du dépôt → **Pages** → Source : `main` / `root`.
3. Le site sera disponible à `https://<ton-user>.github.io/frahlweb/`.

## Configurer Firebase

1. Va sur https://console.firebase.google.com et crée un projet (gratuit).
2. Ajoute une application **web** au projet : tu obtiens un objet `firebaseConfig`
   (apiKey, authDomain, projectId, etc.).
3. Ouvre `index.html`, cherche le bloc `const firebaseConfig = { ... }` tout en
   bas du fichier, et remplace les valeurs `TON_...` par les tiennes.
4. Dans la console Firebase, active **Firestore Database** (mode production ou test).

Le site utilise deux collections Firestore, créées automatiquement au premier ajout :

- **`sites`** — le registre des sites (utilisé par la recherche et la section
  "Sites les plus utilisés") : `nom`, `description`, `lien`, `motsCles` (tableau),
  `icone` (URL, optionnel), `dateAjout`.
- **`actualites`** — les actualités : `nom` (site source), `image` (URL,
  optionnel), `titre`, `description`, `lien`, `dateAjout`.

## Panel admin

Le petit point discret en bas de la barre latérale ouvre le panel admin (ajout /
suppression de sites et d'actualités). **Attention** : pour l'instant ce bouton
n'est pas protégé par un vrai compte — n'importe qui trouvant le bouton peut
écrire dans Firestore. Deux options avant de publier le lien du site à tout le
monde :

- Restreins les règles Firestore (lecture publique, écriture désactivée) le
  temps que la vraie connexion (bouton "Connexion") soit branchée sur Firebase
  Auth.
- Ou limite l'écriture aux comptes que tu whitelist manuellement une fois
  l'authentification ajoutée.

Exemple de règles Firestore restrictives en attendant l'authentification :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;
      allow write: if false; // à activer une fois l'auth admin en place
    }
  }
}
```

## À faire ensuite

- Brancher le bouton "Connexion" sur Firebase Authentication (comptes citoyens).
- Protéger le panel admin par un vrai contrôle d'accès une fois l'auth en place.
- Rendre "Qualité de l'air" et la météo dynamiques (API ou données du serveur).
- Définir comment "Sites les plus utilisés" est calculé (tri manuel, compteur de
  clics, etc.) — pour l'instant la section liste tous les sites du registre.
