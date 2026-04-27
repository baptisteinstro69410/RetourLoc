# Contrôle périodique – Scan BJONG + Batteries

Application HTML autonome pour **scanner des SN (Code 128)**, lire les **fichiers de contrôle** (Excel / CSV / TXT), vérifier si le **dernier contrôle** est encore valide selon un **seuil personnalisable en jours**, puis **exporter l'historique** en CSV.

## Fichiers fournis

- `index.html` → application web à déployer sur GitHub Pages
- `README.md` → ce guide

## Fonctionnalités

- Scan **Code 128** via **Quagga2**
- Lecture de fichiers **`.xlsx` / `.xls` / `.csv` / `.txt`**
- Support **multi-feuilles** dans Excel
- Détection automatique des colonnes :
  - **SN** (`NS Batterie`, `NS`, `SN`, `SN (V)`...)
  - **Date de contrôle** (`Heure de début`, `DATE`, `Date contrôle`...)
- Gestion du **`V` initial optionnel** sur les SN
- Gestion de plusieurs structures de fichiers (exports Forms / feuilles simplifiées)
- Seuil de validité **personnalisable** (ex. 300 jours)
- Affichage :
  - **vert** = contrôle valide
  - **rouge** = contrôle expiré / absent / SN inconnu
- Overlay persistant jusqu'au prochain scan
- Commentaire rapide sur chaque scan
- Bouton **RAS**
- Bouton **Annuler dernier scan**
- Bouton **Flush base scannée**
- Bouton **Vider références**
- Export **CSV enrichi**
- Partage natif si disponible sur le navigateur
- Torche si disponible sur l'appareil

## Format attendu des fichiers de contrôle

L'application essaie d'identifier automatiquement les bonnes colonnes.

### Cas 1 – Export Microsoft Forms
Exemple de colonnes reconnues :

- `Heure de début`
- `NS Batterie`

### Cas 2 – Feuille simplifiée
Exemple de colonnes reconnues :

- `NS`
- `NS (V)`
- `DATE`

### Règles de lecture

- si plusieurs feuilles existent, **elles sont toutes lues**
- si un même SN apparaît plusieurs fois, **la date de contrôle la plus récente** est conservée
- si le SN scanné commence par `V`, l'application compare aussi sans ce `V`

## Règle métier

Pour chaque SN scanné :

- si la **date du dernier contrôle** est trouvée
- et si l'**âge du contrôle** est **≤ seuil**
  - statut = **OK**
- sinon
  - statut = **A CONTRÔLER**

Le seuil est modifiable dans l'interface.

## Déploiement GitHub Pages

### Option simple

1. renommer le fichier HTML final en **`index.html`**
2. déposer `index.html` à la racine du dépôt GitHub
3. committer / pousser
4. activer **GitHub Pages** sur la branche souhaitée

### Important

La caméra ne fonctionne que si l'application est servie en :

- **HTTPS**
- ou **localhost**

## Utilisation

### 1. Charger les fichiers de contrôle

- cliquer sur **Charger fichier BJONG** et/ou **Charger fichier Batteries**
- attendre le message de statut indiquant le nombre de SN reconnus

### 2. Régler le seuil

- saisir le nombre de jours dans **Seuil validité contrôle (jours)**
- cliquer sur **Appliquer**

### 3. Scanner

- cliquer sur **Autoriser & scanner**
- présenter le code dans la zone bleue
- le résultat s'affiche en plein écran

### 4. Saisie manuelle

- saisir le SN dans le champ **Barcode**
- cliquer sur **Valider manuel**

### 5. Commenter / corriger

- saisir une note dans l'overlay ou dans le champ **Notes**
- utiliser **RAS** si aucun commentaire n'est nécessaire
- utiliser **Annuler dernier scan** si besoin

### 6. Exporter

- cliquer sur **Exporter CSV**
- ou **Partager** si le navigateur prend en charge le partage de fichiers

## Colonnes exportées dans le CSV

L'export contient :

- `SN référence`
- `Barcode réel scanné`
- `Saisi / scanné`
- `Statut`
- `Motif`
- `Date contrôle`
- `Âge contrôle (jours)`
- `Seuil (jours)`
- `Source`
- `Date scan`
- `Notes`

## Boutons de maintenance

### Flush base scannée
Vide immédiatement l'historique des scans enregistrés localement.

### Vider références
Décharge les références lues depuis les fichiers BJONG / Batteries et remet les sélecteurs de fichiers à zéro.

## Compatibilité

- iPhone / Safari : OK si accès via HTTPS
- Android / Chrome : OK si accès via HTTPS
- PC : OK avec webcam si navigateur compatible

## Dépannage

### La caméra ne démarre pas

Vérifier :

- que l'application est ouverte en **HTTPS**
- que l'autorisation caméra est acceptée
- que le navigateur autorise la caméra

### Le SN est trouvé mais la date semble absente

Vérifier :

- que le fichier contient bien une colonne de date exploitable
- que la feuille Excel utilisée contient bien le SN
- que la date est dans un format interprétable

### Le scan est rouge alors que le contrôle est récent

Vérifier :

- le **seuil** configuré
- le **format de date** du fichier
- la présence d'une date plus récente sur une autre feuille

## Version recommandée

Utiliser la version HTML finale fournie avec le correctif :

- lecture multi-feuilles
- détection de colonnes
- gestion des dates
- correction RAS / flush / vider références

---

Si besoin, une évolution possible est d'ajouter :

- filtre **uniquement à contrôler**
- export **des seuls SN expirés**
- affichage du **nom de la feuille utilisée** pour chaque correspondance
