cat > README.md <<'EOF'
# Lab Android Security — Analyse de l’application DIVA avec Drozer

## Présentation du lab

Ce lab a pour objectif de réaliser une première analyse de sécurité Android sur l’application vulnérable **DIVA** à l’aide de l’outil **Drozer**.

L’objectif principal est d’identifier les informations importantes de l’application Android, notamment :

- le nom du package ;
- les permissions utilisées ;
- les activités exposées ;
- le contenu du fichier Manifest ;
- les Content Providers disponibles ;
- les premiers risques de sécurité visibles.

L’application étudiée est :

`jakhar.aseem.diva`

L’outil utilisé pour l’analyse est :

`Drozer`

---

## Organisation des fichiers du repository

Les captures d’écran doivent être placées directement dans le même dossier que le fichier `README.md`.

Structure attendue du projet :

    projet-lab-android/
    ├── README.md
    ├── image.png
    ├── pic0.png
    ├── pic1.png
    ├── pic2.png
    ├── pic3.png
    ├── pic4.png
    └── pic5.png

Important : il faut garder exactement les mêmes noms de fichiers pour que les images s’affichent correctement dans GitHub.

---

## 1. Architecture générale du lab

Le lab est composé de plusieurs éléments :

- un ordinateur Windows ;
- un terminal CMD ou PowerShell ;
- Android Studio avec un émulateur Android ;
- ADB pour communiquer avec l’émulateur ;
- Drozer Agent installé sur l’émulateur ;
- l’application vulnérable DIVA ;
- Drozer utilisé depuis le terminal.


---

## 2. Installation de Drozer Agent

La première étape consiste à installer l’application **Drozer Agent** sur l’émulateur Android.

Commande utilisée :

    adb install drozer-agent.apk

Après l’exécution de la commande, le terminal affiche :

    Performing Streamed Install
    Success

Cela signifie que l’installation de Drozer Agent a été réalisée correctement.

Emplacement du screen dans le README :

Mettre ici la capture qui montre l’installation réussie de `drozer-agent.apk`.

<img width="1461" height="259" alt="pic0" src="https://github.com/user-attachments/assets/5be050ea-9663-4018-a654-0914bd866ae2" />


---

## 3. Lancement de Drozer Agent sur l’émulateur

Après l’installation, Drozer Agent est lancé directement depuis l’émulateur Android.

L’interface affiche :

- le logo Drozer ;
- l’option Embedded Server ;
- le port utilisé : `31415` ;
- le bouton permettant d’activer ou désactiver le serveur.

Cette étape confirme que Drozer Agent est bien installé et prêt à être utilisé pour la communication avec Drozer côté terminal.

Emplacement du screen dans le README :

Mettre ici la capture qui montre l’interface Drozer Agent ouverte sur l’émulateur.

<img width="393" height="708" alt="pic1" src="https://github.com/user-attachments/assets/b76d22c5-f096-436c-a88a-778b45323d42" />


---

## 4. Identification de l’application cible

L’application analysée dans ce lab est **DIVA**.

Nom du package :

    jakhar.aseem.diva

DIVA est une application volontairement vulnérable utilisée dans les labs de sécurité Android. Elle permet de pratiquer l’analyse des failles dans un environnement contrôlé.

---

## 5. Collecte des informations du package

Pour afficher les informations générales de l’application, la commande suivante est utilisée :

    run app.package.info -a jakhar.aseem.diva

Cette commande permet d’obtenir plusieurs informations importantes :

- le nom du package ;
- le nom de l’application ;
- le processus utilisé ;
- la version ;
- le répertoire des données ;
- le chemin de l’APK ;
- l’UID ;
- les permissions utilisées par l’application.

Les permissions observées sont :

    android.permission.WRITE_EXTERNAL_STORAGE
    android.permission.READ_EXTERNAL_STORAGE
    android.permission.INTERNET
    android.permission.ACCESS_MEDIA_LOCATION

Ces permissions sont importantes car elles montrent que l’application peut accéder au stockage externe et utiliser Internet.

Emplacement du screen dans le README :

Mettre ici la capture qui montre le résultat de la commande `app.package.info`.

<img width="1677" height="938" alt="pic2" src="https://github.com/user-attachments/assets/1df2ea76-8a88-4e37-bffb-60c0fa21ea71" />


---

## 6. Analyse des activités exposées

Pour afficher les activités déclarées dans l’application, la commande suivante est utilisée :

    run app.activity.info -a jakhar.aseem.diva -i

Cette commande permet de lister les activités Android disponibles dans l’application.

Activités observées :

    jakhar.aseem.diva.MainActivity
    jakhar.aseem.diva.APICredsActivity
    jakhar.aseem.diva.APICreds2Activity

L’activité principale contient :

    android.intent.action.MAIN
    android.intent.category.LAUNCHER

Cela signifie que cette activité correspond à l’écran principal lancé quand l’utilisateur ouvre l’application.

D’autres activités utilisent des actions personnalisées :

    jakhar.aseem.diva.action.VIEW_CREDS
    jakhar.aseem.diva.action.VIEW_CREDS2

Ces éléments sont importants parce qu’ils peuvent représenter des points d’entrée à analyser dans la suite du lab.

Emplacement du screen dans le README :

Mettre ici la capture qui montre la liste des activités exposées.

<img width="745" height="512" alt="pic4" src="https://github.com/user-attachments/assets/1bb835b4-ba97-4ecf-abd5-bb3e58bb4b8e" />


---

## 7. Analyse du fichier Android Manifest

Le fichier Manifest est analysé avec la commande suivante :

    run app.package.manifest jakhar.aseem.diva

Le fichier Manifest contient les informations principales de l’application Android :

- le nom du package ;
- la version ;
- le SDK minimum ;
- le SDK cible ;
- les permissions ;
- les activités ;
- les paramètres de sécurité.

Informations importantes observées :

    package="jakhar.aseem.diva"
    minSdkVersion="15"
    targetSdkVersion="23"
    debuggable="true"
    allowBackup="true"
    supportsRtl="true"

Deux éléments sont particulièrement importants pour la sécurité :

    debuggable="true"
    allowBackup="true"

Le paramètre `debuggable="true"` signifie que l’application est en mode débogage. Ce mode ne doit pas être activé dans une application de production.

Le paramètre `allowBackup="true"` signifie que les données de l’application peuvent être sauvegardées. Si des données sensibles sont stockées, cela peut représenter un risque.

Emplacement du screen dans le README :

Mettre ici la capture qui montre le fichier Manifest de l’application DIVA.

<img width="604" height="651" alt="pic3" src="https://github.com/user-attachments/assets/a1496c7c-b381-4b12-8414-2c545e18e995" />


---

## 8. Recherche des Content Providers

Pour rechercher les Content Providers de l’application, la commande suivante est utilisée :

    run app.provider.finduri jakhar.aseem.diva

Résultat obtenu :

    content://jakhar.aseem.diva.provider.notesprovider
    content://jakhar.aseem.diva.provider.notesprovider/notes/
    content://jakhar.aseem.diva.provider.notesprovider/notes
    content://jakhar.aseem.diva.provider.notesprovider/

Ces résultats montrent que l’application possède un provider lié aux notes.

Un Content Provider est un composant Android qui peut permettre à une application de partager des données avec d’autres applications. Dans un audit de sécurité, il faut vérifier si ce provider est bien protégé par des permissions.

Emplacement du screen dans le README :

Mettre ici la capture qui montre les URI trouvés avec `app.provider.finduri`.

<img width="831" height="217" alt="pic5" src="https://github.com/user-attachments/assets/199c8b69-eeb6-4e85-8059-9ecf1e6046ab" />


---

## 9. Tableau récapitulatif des résultats

| Élément analysé | Résultat obtenu |
|---|---|
| Application cible | DIVA |
| Package | `jakhar.aseem.diva` |
| Outil utilisé | Drozer |
| Agent Android | Drozer Agent |
| Port Drozer Agent | `31415` |
| Permissions sensibles | Stockage externe, Internet |
| Activités exposées | MainActivity, APICredsActivity, APICreds2Activity |
| Manifest analysé | Oui |
| Mode debug | Activé |
| allowBackup | Activé |
| Content Provider détecté | Notes Provider |

---

## 10. Analyse des risques observés

Les premiers résultats permettent d’identifier plusieurs points de vigilance :

| Observation | Risque possible |
|---|---|
| Permissions de stockage externe | Accès possible à des fichiers sensibles |
| Permission Internet | Communication réseau possible |
| `debuggable="true"` | Application plus facile à analyser ou manipuler |
| `allowBackup="true"` | Sauvegarde possible de données de l’application |
| Activités exposées | Accès possible à certaines interfaces internes |
| Content Provider détecté | Risque d’accès non autorisé aux données si mal protégé |

---

## 11. Mapping OWASP MASVS / MASTG

| Élément observé | Catégorie OWASP |
|---|---|
| Permissions de stockage | MASVS-STORAGE |
| Content Provider exposé | MASVS-PLATFORM |
| Activités accessibles par Intent | MASVS-PLATFORM |
| Application en mode debug | MASVS-RESILIENCE |
| Sauvegarde activée | MASVS-STORAGE |

---

## 12. Remédiations recommandées

Pour améliorer la sécurité de l’application, il est recommandé de :

- désactiver le mode debug avant la mise en production ;
- mettre `allowBackup` à `false` si la sauvegarde n’est pas nécessaire ;
- limiter les permissions au strict minimum ;
- protéger les activités sensibles ;
- protéger les Content Providers avec des permissions ;
- éviter de stocker des données sensibles en clair ;
- vérifier les intents exposés ;
- appliquer les bonnes pratiques OWASP MASVS.

---

## 13. Emplacement exact des screens dans le README

| Nom du screen | Où le mettre dans le README |
|---|---|
| `image.png` | Section 1 : Architecture générale du lab |
| `pic0.png` | Section 2 : Installation de Drozer Agent |
| `pic1.png` | Section 3 : Lancement de Drozer Agent sur l’émulateur |
| `pic2.png` | Section 5 : Collecte des informations du package |
| `pic4.png` | Section 6 : Analyse des activités exposées |
| `pic3.png` | Section 7 : Analyse du fichier Android Manifest |
| `pic5.png` | Section 8 : Recherche des Content Providers |

---

## 14. Conclusion

Ce lab a permis de réaliser une première analyse de l’application Android DIVA avec Drozer.

Les étapes réalisées sont :

- installation de Drozer Agent ;
- lancement de Drozer Agent sur l’émulateur ;
- identification du package DIVA ;
- collecte des informations générales ;
- analyse des permissions ;
- analyse des activités ;
- analyse du Manifest ;
- recherche des Content Providers ;
- identification des premiers risques de sécurité.

Cette phase représente une étape importante avant de passer à des tests plus avancés dans un environnement de laboratoire.

---

## 15. Auteur

Réalisé par :

    HMAMI Mohamed

---

## 16. Remarque importante

Ce travail est réalisé uniquement dans un environnement de laboratoire à des fins pédagogiques.

L’application DIVA est volontairement vulnérable et doit être utilisée seulement pour apprendre les bases de l’audit de sécurité Android.
EOF

