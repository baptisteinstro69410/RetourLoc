# RetourLoc

Application web statique pour scanner les retours de location, contrôler les numéros de série et vérifier la validité du dernier contrôle périodique depuis un fichier Excel unique.

## Objectif

RetourLoc permet de :

- scanner des codes-barres Code 128 depuis la caméra d’un téléphone ;
- charger un seul fichier Excel regroupant les contrôles BJONG et Batteries ;
- rechercher chaque numéro de série scanné dans ce fichier ;
- vérifier que le dernier contrôle date de moins de 300 jours par défaut ;
- personnaliser le seuil de contrôle en jours ;
- afficher immédiatement un résultat visuel OK / À contrôler ;
- ajouter une note ou valider RAS ;
- archiver localement les scans ;
- exporter ou partager un fichier CSV.

## Fichiers du dépôt

À placer à la racine du dépôt GitHub Pages :

```text
index.html
README.md
```

Aucun build n’est nécessaire.

## Déploiement GitHub Pages

1. Ouvrir le dépôt GitHub.
2. Remplacer ou ajouter le fichier `index.html` à la racine.
3. Ajouter ce fichier `README.md` à la racine.
4. Commit / push sur la branche `main`.
5. Dans GitHub : `Settings > Pages`.
6. Configurer :
   - Source : `Deploy from a branch`
   - Branch : `main`
   - Folder : `/ (root)`
7. Attendre la publication.

URL attendue :

```text
https://baptisteinstro69410.github.io/RetourLoc/
```

Pour forcer le rafraîchissement après déploiement :

```text
https://baptisteinstro69410.github.io/RetourLoc/?v=11
```

## Utilisation

### 1. Charger le fichier Excel unique

Cliquer sur le champ :

```text
Fichier Excel unique contrôles BJONG + Batteries
```

Puis sélectionner le fichier Excel contenant les contrôles.

L’application lit toutes les feuilles du classeur.

### 2. Règle de lecture du fichier Excel

L’application tente de détecter automatiquement les colonnes utiles à partir des en-têtes.

Si les colonnes ne sont pas détectées automatiquement, le fallback appliqué est :

| Donnée | Colonne Excel |
|---|---:|
| Numéro de série | F |
| Date de contrôle | B |

La date peut être au format :

```text
20/01/2026 15:20:46
```

ou dans un format date Excel reconnu.

### 3. Gestion des numéros de série

Les numéros de série sont normalisés avant comparaison.

Règles appliquées :

- suppression des espaces ;
- conversion en majuscules ;
- gestion du préfixe `V` facultatif devant les numéros numériques.

Exemples considérés équivalents :

```text
V202100132125
202100132125
```

### 4. Personnaliser le seuil de contrôle

Le champ `Seuil contrôle` permet de modifier la périodicité acceptée.

Valeur par défaut :

```text
300 jours
```

Règle appliquée :

- dernier contrôle ≤ seuil : `OK` ;
- dernier contrôle > seuil : `À CONTRÔLER` ;
- date absente : `À CONTRÔLER` ;
- SN absent du fichier : `À CONTRÔLER`.

### 5. Scanner un code-barres

Cliquer sur :

```text
Autoriser & scanner
```

Puis autoriser l’accès caméra si demandé.

L’application utilise Quagga pour lire les codes-barres Code 128.

Le repère de scan est affiché au centre de la zone caméra :

- rectangle de visée ;
- ligne rouge centrale ;
- zone de lecture visuelle.

### 6. Résultat après scan

Après chaque scan, l’application affiche un écran de résultat :

- vert : `OK` ;
- rouge : `À CONTRÔLER`.

L’écran affiche :

- le SN normalisé ;
- le statut ;
- la raison du statut ;
- le type détecté ;
- la date du dernier contrôle ;
- l’âge du contrôle en jours.

### 7. Notes et RAS

Depuis l’écran de résultat, il est possible de :

- saisir une note matériel ;
- cliquer sur `RAS` ;
- annuler le scan ;
- lancer le scan suivant.

Les notes sont enregistrées dans l’historique local et dans l’export CSV.

### 8. Saisie manuelle

Un SN peut être saisi manuellement dans le champ :

```text
Saisie manuelle SN
```

Puis validé avec :

```text
Valider manuel
```

La même logique de contrôle est appliquée que pour un scan caméra.

## Export CSV

Le bouton `Exporter CSV` télécharge un fichier CSV.

Le bouton `Partager` utilise le partage natif du téléphone si disponible. Sinon, un export CSV classique est lancé.

Le nom du fichier exporté est basé sur :

- le champ client ;
- le numéro de bon de réception ;
- la date du jour.

Format du nom :

```text
RetourLoc_CLIENT_BR_AAAA-MM-JJ.csv
```

Colonnes exportées :

```text
Date scan
Client
Bon reception
Barcode reel scanne
SN normalise
Statut
Raison
Type controle
Date dernier controle
Age jours
Scan manuel
Notes SN
Notes globales
```

## Stockage local

L’historique des scans est stocké localement dans le navigateur via `localStorage`.

Conséquences :

- l’historique reste disponible après fermeture / réouverture de la page ;
- l’historique ne suit pas automatiquement sur un autre appareil ;
- vider les données du navigateur supprime l’historique.

## Actions maintenance

Les actions sensibles sont regroupées dans la section :

```text
Zone maintenance / actions dangereuses
```

Cette zone contient :

- `Flush base scannée` ;
- `Vider références`.

Le bouton `Flush base scannée` est volontairement éloigné des boutons `Exporter CSV` et `Partager` afin d’éviter les mauvais clics.

Une confirmation est demandée avant suppression de la base scannée.

## Mode clair / mode sombre

L’application propose une case :

```text
Mode sombre
```

Le mode clair est le mode par défaut.

Le choix est conservé localement dans le navigateur.

## Compatibilité

Recommandé :

- iPhone avec Safari récent ;
- Chrome mobile ;
- accès HTTPS via GitHub Pages.

Important : la caméra ne fonctionne pas correctement si la page est ouverte en HTTP ou depuis un fichier local.

## Dépendances externes

L’application charge les bibliothèques suivantes depuis CDN :

- Quagga2 pour le scan Code 128 ;
- SheetJS / XLSX pour la lecture Excel.

Un accès Internet est nécessaire au chargement initial de la page pour récupérer ces bibliothèques.

## Limites connues

- La torche dépend du support navigateur / appareil.
- La lecture caméra dépend des autorisations iOS / Android.
- Le scan peut nécessiter un bon éclairage et un code-barres horizontal.
- Le fichier Excel doit contenir des SN exploitables et des dates de contrôle reconnues.
- Les données restent locales au navigateur.

## Recommandations terrain

- Utiliser l’URL GitHub Pages en HTTPS.
- Après déploiement, forcer le cache avec `?v=11`.
- Garder le code-barres horizontal dans le rectangle de visée.
- Remplir le champ client et le bon de réception avant export.
- Utiliser `RAS` pour accélérer la saisie des notes sans anomalie.
- En cas d’erreur de scan, utiliser `Annuler ce scan`, `Annuler dernier scan` ou le bouton `Annuler` sur la ligne concernée.

## Version documentée

README correspondant à la version `index.html` v11.

Fonctions principales validées :

- fichier Excel unique ;
- lecture toutes feuilles ;
- fallback SN colonne F / date colonne B ;
- seuil personnalisable ;
- scan Quagga Code 128 ;
- affichage OK / À contrôler ;
- notes et RAS ;
- export / partage CSV ;
- mode clair / sombre ;
- zone maintenance séparée ;
- bouton flush éloigné des actions de partage / export.
