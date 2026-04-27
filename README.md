# RetourLoc

Application web statique pour **scanner les retours de location** (BJONG + batteries), vérifier la présence du **SN** dans les fichiers de référence, afficher un résultat **plein écran vert / rouge**, archiver localement les scans, puis **exporter / partager** un CSV.

## Fonctions principales

- **Scan Code 128** via caméra (Quagga2)
- **Chargement de 2 sources** :
  - fichier **BJONG**
  - fichier **Batteries**
- Lecture des SN depuis les **colonnes A ou B** de la première feuille / du CSV
- Résultat visuel :
  - **Vert = OK**
  - **Rouge = À contrôler**
- L’écran rouge / vert **reste affiché jusqu’au prochain scan**
- **Archivage local** des SN scannés
- Champ **Notes** pour commentaire sur le matériel
- **Annuler le dernier scan**
- **Annuler une ligne précise**
- **Flush base scannée**
- **Export CSV**
- **Partage** (si le téléphone supporte Web Share)

---

## Fichiers à mettre dans le dépôt GitHub Pages

À la racine du repo :

- `index.html`
- `README.md`

Aucun build n’est nécessaire.

---

## Déploiement GitHub Pages

1. Créer / utiliser le repo **`RetourLoc`**
2. Déposer `index.html` et `README.md` à la racine
3. Dans **Settings > Pages**
4. Choisir **Deploy from a branch**
5. Sélectionner :
   - **Branch** : `main`
   - **Folder** : `/ (root)`
6. Enregistrer
7. Attendre la publication

URL attendue :

```text
https://baptisteinstro69410.github.io/RetourLoc/
```

---

## Utilisation

### 1. Charger les références
- Charger le fichier **BJONG**
- Charger le fichier **Batteries**

L’application fusionne les SN en mémoire dans une seule base de référence.

### 2. Scanner
- Cliquer sur **Autoriser & scanner**
- Présenter le code-barres dans la zone bleue
- Si le SN est connu : **OK**
- Sinon : **À contrôler**

### 3. Ajouter une note
Dans le champ **Notes**, saisir par exemple :
- câble manquant
- choc boîtier
- batterie absente
- RAS

### 4. Archiver
Chaque scan est ajouté à la liste locale avec :
- SN
- statut
- source
- date / heure
- notes

### 5. Exporter / partager
- **Exporter CSV** pour intégrer le résultat dans un bon de réception
- **Partager** pour envoyer via mail / Teams / WhatsApp si disponible

---

## Boutons disponibles

- **Autoriser & scanner** : démarre la caméra
- **Arrêter** : arrête le scan
- **Torche** : active la lumière si supportée
- **Scanner suivant** : lance un nouveau scan et efface l’écran rouge / vert précédent
- **Annuler dernier scan** : supprime le dernier scan archivé
- **Valider manuel** : enregistre le SN saisi manuellement
- **Exporter CSV** : télécharge le fichier CSV
- **Partager** : partage le CSV si supporté
- **Flush base scannée** : vide uniquement l’historique des scans
- **Vider références** : décharge les références BJONG / Batteries

---

## Format des fichiers sources

### Fichiers acceptés
- `.csv`
- `.txt`
- `.xlsx`
- `.xls`

### Hypothèse de lecture
L’application lit les **colonnes A et B** de la **première feuille**.

Si tes SN sont ailleurs, il faudra adapter le code.

---

## Format du CSV exporté

Colonnes exportées :

```text
SN ; Statut ; Source ; Date ; Notes
```

---

## Stockage local

Les scans sont stockés dans le navigateur via `localStorage`.

Conséquences :
- si tu vides les données du navigateur, l’historique disparaît
- si tu changes d’appareil, l’historique ne suit pas
- le **flush** vide uniquement la base scannée locale

---

## Recommandations terrain

- Utiliser de préférence **Chrome mobile** ou **Safari iPhone récent**
- Garder le code-barres **horizontal**
- Faire remplir le cadre bleu par le code
- Si scan erroné :
  - **Annuler dernier scan**
  - ou **Annuler** sur la ligne concernée

---

## Limites connues

- Le chargement du moteur de scan **Quagga2** nécessite un accès Internet au chargement initial
- La torche n’est pas disponible sur tous les appareils / navigateurs
- Les fichiers Excel sont lus sur la **première feuille uniquement**

---

## Évolutions possibles

- ajout d’un champ **client**
- ajout d’un champ **n° bon de réception**
- filtre **uniquement à contrôler**
- export enrichi pour impression directe
- synchronisation SharePoint / Excel / Dataverse
