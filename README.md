# RetourLoc

Application web statique pour scanner des retours de location, vérifier le dernier contrôle d’un matériel depuis un fichier Excel, puis partager/exporter la base scannée au format CSV.

## Fichiers du dépôt

```text
index.html
README.md
```

Aucun build n’est nécessaire.

## Déploiement

Déposer `index.html` à la racine du dépôt GitHub Pages.

URL de test :

```text
https://baptisteinstro69410.github.io/RetourLoc/
```

En cas de cache navigateur après modification, ajouter un paramètre temporaire :

```text
https://baptisteinstro69410.github.io/RetourLoc/?refresh=1
```

## Fonctionnement

1. Charger le fichier Excel de contrôles.
2. Vérifier que le référentiel affiche des SN détectés.
3. Scanner le code-barres du matériel.
4. L’application affiche `OK` ou `À CONTRÔLER`.
5. Utiliser `Partager` pour envoyer le CSV des scans.

## Fichier Excel attendu

L’application lit toutes les feuilles du classeur.

Elle recherche automatiquement :

- une colonne SN ;
- une colonne date de contrôle ;
- si disponible, une colonne résultat de contrôle.

Colonnes SN reconnues notamment :

```text
SN_NORMALISE
SN
NS
NS BJONG
NS Batterie
NS Doppler H/V
NS RADAR
N° série
Barcode
```

Colonnes date reconnues notamment :

```text
DATE_CONTROLE
Date contrôle
Dernier contrôle
Heure de début
Date
Timestamp
```

Colonnes résultat reconnues notamment :

```text
RESULTAT_CONTROLE
Résultat contrôle
Résultat Autotest
Résutat
Résultat test H/V
Résultat test Hauteur
```

Si aucune colonne résultat n’est trouvée, le contrôle est considéré `OK` par défaut.

## Règle de décision

Un matériel est affiché `OK` uniquement si :

```text
SN trouvé
Dernier contrôle trouvé
Résultat contrôle = OK
Âge du contrôle <= seuil configuré
```

Sinon, le matériel est affiché :

```text
À CONTRÔLER
```

Résultats considérés comme OK :

```text
OK
PASS
CONFORME
```

Tout autre résultat est considéré comme `FAIL`.

## Normalisation SN

Avant comparaison, l’application :

- supprime les espaces ;
- met en majuscules ;
- supprime le préfixe `V` si le SN commence par `V` suivi d’un chiffre.

Exemple :

```text
V202100137747 -> 202100137747
```

## Interface terrain

La barre rapide en bas contient :

```text
Scanner | Suivant | Partager
```

Le bouton `Partager` utilise le partage natif du téléphone avec le CSV généré.

Le bouton `Exporter CSV` reste disponible dans la section `Base scannée` comme solution de secours.

## Données locales

Les scans sont stockés localement dans le navigateur via `localStorage`.

Ils restent disponibles après fermeture/réouverture de la page, tant que les données du navigateur ne sont pas supprimées.

## Zone maintenance

La section `Zone maintenance / actions dangereuses` contient :

- Mode sombre ;
- Flush base scannée ;
- Vider références.

Le bouton `Flush base scannée` supprime l’historique local après confirmation.

## Compatibilité

Recommandé :

- GitHub Pages en HTTPS ;
- iPhone Safari récent ;
- Chrome mobile récent ;
- navigateur compatible caméra ;
- connexion Internet au premier chargement pour récupérer les bibliothèques externes.

## Dépendances

L’application charge depuis CDN :

```text
Quagga2
SheetJS / XLSX
```

## Notes

- La caméra est gérée par Quagga dans la zone `#reader`.
- Le fichier Excel n’est pas envoyé sur un serveur ; il est lu localement par le navigateur.
- Le CSV partagé/exporté est généré localement.
