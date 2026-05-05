# RetourLoc

Application web statique de retour location permettant de scanner des numéros de série, de vérifier le dernier contrôle périodique depuis un fichier Excel unique, puis d’archiver/exporter les scans réalisés sur le terrain.

## Version documentée

README correspondant à :

```text
index.html v16
```

Cette version corrige notamment :

- le stockage correct du SN scanné ;
- l’affichage correct du `Barcode réel` dans la base scannée ;
- le nom du fichier CSV partagé/exporté ;
- le bouton rapide `Partager` en bas d’écran ;
- la lecture robuste du fichier `TEST BJONG.xlsx` avec `NS BJONG` ;
- la conservation de la caméra Quagga sur `#reader` ;
- le mode sombre déplacé dans la zone maintenance.

---

## Objectif de l’application

RetourLoc sert à :

- charger un fichier Excel de contrôles ;
- scanner un code-barres Code 128 avec la caméra ;
- normaliser le SN scanné ;
- rechercher le SN dans le référentiel Excel ;
- récupérer le dernier contrôle connu ;
- vérifier la validité selon un seuil en jours ;
- tenir compte du résultat de contrôle `OK` ou `FAIL` ;
- afficher immédiatement un écran vert ou rouge ;
- archiver les scans localement ;
- partager ou exporter un CSV des scans.

---

## Fichiers attendus dans le dépôt

À la racine du dépôt GitHub Pages :

```text
index.html
README.md
```

Aucun build n’est nécessaire.

---

## Déploiement GitHub Pages

1. Ouvrir le dépôt GitHub.
2. Remplacer le fichier `index.html` par la version v16.
3. Ajouter ou remplacer le fichier `README.md`.
4. Commit / push sur la branche utilisée par GitHub Pages.
5. Attendre la publication GitHub Pages.
6. Tester avec cache forcé :

```text
https://baptisteinstro69410.github.io/RetourLoc/?v=16
```

---

## Utilisation terrain

### 1. Charger le fichier Excel

Cliquer sur :

```text
Fichier Excel unique contrôles
```

Puis sélectionner le fichier `.xlsx` de contrôles.

Après chargement, l’application descend automatiquement vers la zone scanner pour éviter de devoir scroller manuellement.

---

### 2. Vérifier le référentiel chargé

Après chargement, l’application affiche des indicateurs :

```text
Référentiel : X SN uniques
Contrôles OK : X
Contrôles FAIL : X
Lignes lues : X
Lignes SN détectées : X
Feuilles utilisées : X
Feuilles ignorées : X
```

Si le référentiel reste à `0 SN uniques`, vérifier :

- que le fichier Excel contient bien une colonne SN détectable ;
- que le fichier Excel contient bien une date de contrôle ;
- que la feuille contient une ligne d’en-tête ;
- que les dates sont exploitables.

---

### 3. Scanner un matériel

Utiliser le bouton :

```text
Autoriser & scanner
```

ou le bouton rapide en bas d’écran :

```text
Scanner
```

Après lecture d’un code-barres, l’application :

1. arrête temporairement la caméra ;
2. normalise le SN ;
3. recherche le SN dans le référentiel ;
4. affiche le résultat ;
5. archive la ligne dans la base scannée locale.

---

### 4. Scanner le matériel suivant

Depuis l’écran de résultat, utiliser :

```text
Scanner suivant
```

ou depuis la barre rapide :

```text
Suivant
```

---

### 5. Annuler un scan

Depuis l’écran de résultat :

```text
Annuler ce scan
```

Comportement attendu en v16 :

- la ligne est supprimée de la base scannée ;
- l’écran résultat se ferme ;
- l’application revient sur la zone scanner ;
- la caméra est relancée automatiquement.

---

## Structure Excel supportée

L’application lit toutes les feuilles du classeur Excel.

Elle recherche automatiquement une ligne d’en-tête contenant à la fois :

- une information SN ;
- une information date.

Les feuilles sans en-tête exploitable sont ignorées.

---

## Colonnes SN détectées

L’application reconnaît notamment :

```text
SN_NORMALISE
SN normalisé
SN normalise
SN
NS
NS BJONG
NS Batterie
NS Doppler H/V
NS RADAR
N° série
N° serie
Numero serie
Numéro série
Serial
Code barre
Barcode
```

---

## Colonnes date détectées

L’application reconnaît notamment :

```text
DATE_CONTROLE
Date contrôle
Date controle
Dernier contrôle
Dernier controle
Heure de début
Heure de debut
Date
Timestamp
```

---

## Colonnes résultat détectées

L’application reconnaît notamment :

```text
RESULTAT_CONTROLE
Résultat contrôle
Resultat controle
Résultat Autotest
Resultat Autotest
Résutat
Résultat
Resultat
Résultat test H/V
Resultat test H/V
Résultat test Hauteur
Resultat test Hauteur
```

Si aucune colonne résultat n’est trouvée, l’application considère par défaut le contrôle comme `OK`.

---

## Normalisation SN

La fonction de normalisation applique :

- suppression des espaces ;
- passage en majuscules ;
- suppression du préfixe `V` si le SN commence par `V` suivi d’un chiffre.

Exemples :

```text
V202100137747 -> 202100137747
202100137747  -> 202100137747
c00040438     -> C00040438
```

---

## Logique OK / À contrôler

Pour qu’un matériel soit affiché `OK`, toutes les conditions suivantes doivent être vraies :

```text
SN trouvé dans le référentiel
Dernier contrôle trouvé
Résultat du dernier contrôle = OK
Âge du dernier contrôle <= seuil défini
```

Sinon, le matériel ressort :

```text
À CONTRÔLER
```

Cas possibles :

```text
SN absent                      -> À CONTRÔLER
Date absente                   -> À CONTRÔLER
Dernier résultat = FAIL         -> À CONTRÔLER
Dernier résultat = OK trop vieux -> À CONTRÔLER
Dernier résultat = OK récent    -> OK
```

---

## Résultats de contrôle reconnus

Sont considérés comme `OK` :

```text
OK
PASS
CONFORME
```

Tout autre contenu est considéré comme :

```text
FAIL
```

---

## Seuil de contrôle

Le seuil par défaut est :

```text
300 jours
```

Le seuil est modifiable dans le champ :

```text
Seuil contrôle
```

---

## Base scannée locale

Chaque scan archivé contient :

```text
Date scan
Barcode réel scanné
SN normalisé
Statut
Raison
Type matériel
Date dernier contrôle
Âge du contrôle
Résultat contrôle
Notes
Client
Bon de réception
Notes globales
Scan manuel oui/non
```

Les données sont stockées localement dans le navigateur via :

```text
localStorage
```

Clé utilisée en v16 :

```text
retourloc_scans_v16
```

---

## Partage et export

### Bouton rapide du bas

La barre rapide contient :

```text
Scanner | Suivant | Partager
```

Le bouton `Partager` utilise le partage natif du téléphone avec le fichier CSV.

Sur iPhone, le système affiche la feuille de partage iOS. L’utilisateur choisit ensuite l’application cible, par exemple Teams, Mail ou Fichiers.

---

### Boutons dans la section Base scannée

La section `Base scannée` contient :

```text
Partager
Exporter CSV
```

`Partager` est prioritaire pour l’usage terrain.

`Exporter CSV` reste disponible comme méthode de secours.

---

## Nom du fichier CSV

Le fichier généré suit le format :

```text
RetourLoc_CLIENT_BR_AAAA-MM-JJ.csv
```

Exemple :

```text
RetourLoc_CLIENT_BR_2026-05-05.csv
```

Si les champs client ou BR sont vides, les valeurs par défaut sont :

```text
CLIENT
BR
```

---

## Colonnes du CSV

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
Resultat controle
Scan manuel
Notes SN
Notes globales
```

---

## Mode sombre

Le mode sombre est disponible dans :

```text
Zone maintenance / actions dangereuses
```

Le choix est mémorisé localement dans le navigateur.

---

## Zone maintenance

La zone maintenance contient :

```text
Mode sombre
Flush base scannée
Vider références
```

Le bouton `Flush base scannée` demande une confirmation avant suppression.

---

## Compatibilité

Recommandé :

- GitHub Pages en HTTPS ;
- iPhone Safari récent ;
- Chrome mobile récent ;
- navigateur compatible caméra `getUserMedia` ;
- accès Internet au premier chargement pour récupérer Quagga2 et SheetJS.

---

## Dépendances externes

L’application charge :

```text
Quagga2
SheetJS / XLSX
```

Depuis CDN.

---

## Points corrigés en v16

### Correction du tableau des scans

La colonne `Barcode réel` n’affiche plus de texte de code comme :

```text
${ escapeHtml(...) }
```

Le rendu est maintenant fait par concaténation HTML sécurisée.

### Correction du nom de fichier

Le partage crée désormais explicitement un fichier :

```text
new File([blob], fileName, { type: 'text/csv' })
```

L’export force aussi :

```text
link.download = getExportFileName()
```

Cela évite les exports nommés `example.com` ou équivalent.

### Conservation caméra

La caméra reste gérée par Quagga avec cible directe :

```text
#reader
```

Pas de pré-ouverture `getUserMedia` séparée.

---

## Recommandations terrain

1. Charger le fichier Excel de contrôle.
2. Vérifier que le référentiel affiche bien des SN uniques.
3. Scanner les matériels un par un.
4. Utiliser `RAS` si aucune remarque.
5. Utiliser `Annuler ce scan` en cas d’erreur immédiate.
6. Utiliser `Partager` pour envoyer le CSV.
7. Utiliser `Exporter CSV` uniquement en secours.
8. Éviter `Flush base scannée` sauf fin de session validée.

---

## Historique des versions récentes

### v16

- correction stockage/rendu barcode ;
- correction nom fichier CSV ;
- code lisible et structuré ;
- partage prioritaire ;
- export secondaire ;
- fonctions scanner et Excel conservées.

### v15

- réorganisation lisible du code ;
- bouton bas `Partager` ;
- mais rendu barcode incorrect corrigé ensuite en v16.

### v14

- remplacement du bouton rapide `Export` par `Partager`.

### v13

- amélioration UX ;
- barre rapide basse ;
- mode sombre déplacé en maintenance ;
- annulation scan relançant la caméra.

### v12

- correction lecture `TEST BJONG.xlsx` ;
- détection `NS BJONG` ;
- lecture `raw:true` ;
- prise en compte résultat OK/FAIL.
