# ☁ CloudUnify Pro — App Android
### Obtenir le .apk sans rien installer

---

## Pourquoi ce détour

Je ne peux pas produire le `.apk` fini moi-même : la compilation Android a besoin du SDK Android, hébergé exclusivement sur les serveurs de Google (`dl.google.com`), explicitement bloqués dans mon environnement. Deux façons de contourner ça, aucune n'installe rien sur votre téléphone.

---

## ✅ Option recommandée — Compilation gratuite via GitHub (depuis votre téléphone)

Un fichier de configuration (`.github/workflows/build-apk.yml`) est déjà inclus dans ce projet : il dit à GitHub de compiler le `.apk` automatiquement sur ses propres serveurs, gratuitement.

**1. Créer un compte GitHub** (si vous n'en avez pas)
→ [github.com/signup](https://github.com/signup), depuis le navigateur de votre téléphone

**2. Créer un nouveau dépôt**
→ Bouton **+** en haut à droite → **New repository** → nommez-le `cloudunify-mobile` → **Create repository**

**3. Uploader les fichiers du projet**
→ Sur la page du dépôt vide : **uploading an existing file** → sélectionnez tous les fichiers extraits du ZIP (dossiers `www/`, `android/`, `.github/`, et les fichiers à la racine) → **Commit changes**
> 📱 Si l'upload en un bloc depuis le téléphone est fastidieux (beaucoup de fichiers imbriqués), empruntez n'importe quel ordinateur 5 minutes juste pour cette étape — la suite se fait entièrement depuis le téléphone.

**4. Laisser la compilation se lancer**
→ Elle démarre automatiquement après l'upload. Onglet **Actions** en haut du dépôt → vous verrez "Build Android APK" en cours (point orange) → patientez 3-5 minutes → coche verte ✅

**5. Télécharger le .apk**
→ Cliquez sur le run terminé → tout en bas de la page, section **Artifacts** → téléchargez `CloudUnify-Pro-debug-apk` (c'est un .zip contenant le .apk)

**6. Installer sur votre téléphone**
→ Décompressez, ouvrez `app-debug.apk` → autorisez "Installer des apps inconnues" si demandé → Installer

---

## Option alternative — Android Studio (si vous avez un PC/Mac)

### Étape 1 — Installer Android Studio

1. Téléchargez [Android Studio](https://developer.android.com/studio) (gratuit)
2. Installez-le en laissant toutes les options par défaut (il installe automatiquement le SDK Android + Gradle tout seul)

## Étape 2 — Ouvrir le projet

1. Extrayez le ZIP fourni
2. Android Studio → **Open** → sélectionnez le dossier `cloudunify-mobile/android`
3. Laissez Android Studio synchroniser le projet (barre de progression en bas, 2-5 minutes la première fois — c'est lui qui télécharge Gradle, pas vous)

## Étape 3 — Compiler le .apk

1. Menu **Build** → **Build App Bundle(s) / APK(s)** → **Build APK(s)**
2. Une fois terminé, cliquez le lien **"locate"** dans la notification en bas à droite
3. Le fichier `app-debug.apk` est prêt

## Étape 4 — Installer sur votre téléphone

**Option A — câble USB :**
1. Activez le mode développeur sur votre téléphone (Paramètres → À propos du téléphone → tapez 7 fois sur "Numéro de build")
2. Paramètres → Options pour développeurs → activez "Débogage USB"
3. Branchez le téléphone → dans Android Studio, cliquez le bouton ▶ **Run** — il installe et lance l'app directement

**Option B — sans câble :**
1. Copiez `app-debug.apk` sur votre téléphone (Drive, email, câble, peu importe)
2. Ouvrez le fichier depuis le téléphone → autorisez "Installer des apps inconnues" si demandé → Installer

---

## Configurer l'authentification (une fois par service, quel que soit le chemin choisi ci-dessus)

L'app mobile réutilise **les mêmes Client ID / App key** que la version PC — inutile de tout recréer. Il faut juste ajouter **une redirection supplémentaire** à chacune des 3 consoles, pour que la connexion puisse revenir vers l'app après la connexion dans le navigateur :

```
cloudunify://oauth-callback
```

| Service | Où l'ajouter |
|---|---|
| **Google Drive** | Google Cloud Console → Clients → votre client existant → URI de redirection autorisées → **+ Ajouter un URI** → collez la valeur ci-dessus |
| **OneDrive** | Azure Portal → votre inscription → Authentification → plateforme "Applications mobiles et de bureau" → **+ Ajouter un URI** → collez la valeur |
| **Dropbox** | dropbox.com/developers/apps → votre app → Settings → OAuth 2 → Redirect URIs → collez la valeur → bouton **Add** |

> ⚠ Pour Google spécifiquement : si l'écran de votre client OAuth existant (type "Application Web") n'affiche aucun champ pour ajouter une URI supplémentaire, c'est probablement qu'il faut passer par un client de type différent. Envoyez-moi une capture de cet écran si ça bloque et je vous dirai exactement quoi faire — on a déjà résolu des blocages similaires ensemble pour la version PC.

Une fois ces 3 redirections ajoutées, ouvrez l'app sur votre téléphone → menu ☰ → "+ Ajouter un compte" sous chaque service → collez le même Client ID (et Secret pour Google) que sur PC.

---

## Ce qui change par rapport à la version PC

| | PC | Mobile |
|---|---|---|
| Authentification | Fenêtre + petit serveur local | Navigateur + retour automatique dans l'app |
| Téléchargement de fichiers | Boîte de dialogue "Enregistrer sous" | Sauvegardé dans Documents + feuille de partage native |
| Navigation | Barre latérale fixe | Menu ☰ coulissant |
| Détails d'un fichier | Panneau latéral fixe | Tiroir qui remonte du bas |
| Bouton retour | — | Le bouton retour Android ferme les fenêtres/tiroirs avant de quitter l'app |

Fonctionnellement identique sinon : mêmes comptes multiples par service, mêmes actions (partager, copier vers un autre cloud, supprimer, uploader).

---

## Limitation actuelle

Un seul .apk "debug" est généré, suffisant pour un usage personnel (sideload). Publier sur le Play Store demanderait en plus : une version "release" signée avec un certificat définitif, une fiche store, et l'inscription au programme développeur Google Play (25$ à vie) — dites-le-moi si vous voulez qu'on prépare cette étape plus tard.
