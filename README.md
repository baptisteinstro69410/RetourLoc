# 📦 RetourLoc / ScanLoc

Application web mobile (HTML autonome) permettant de :
- scanner des codes-barres (SN matériel)
- vérifier automatiquement le statut de contrôle périodique
- gérer les opérations : retour, expédition, échange, inventaire
- générer un export CSV exploitable (Power Automate / SharePoint)

---

# 🚀 Fonctionnalités

## 🔍 Scan matériel
- Scan caméra (Quagga2)
- Compatible iPhone / Android
- Saisie manuelle possible
- Anti double scan

## 📊 Vérification automatique
- Chargement d’un fichier Excel
- Détection automatique des colonnes :
  - SN
  - Date contrôle
  - Résultat
  - Type matériel
- Calcul du statut :
  - ✅ OK → conforme
  - ❌ À CONTRÔLER → hors délai / défaut / absent

---

## 🎯 Gestion des destinations

Obligatoire avant export.

Choix disponibles :
- 📦 Expedition
- 🔁 Echange
- ✅ Retour
- 📋 Inventaire

Effets :
- modifie automatiquement le libellé du champ (BL / BR / ECH / INV)
- affiché dans l’interface (badge bas écran)
- enregistré dans chaque ligne de scan
- intégré au nom du fichier exporté

---

## 📁 Export CSV

### Structure

Colonnes générées :

- Date scan
- Destination
- Client
- Référence document (BR / BL / ECH / INV)
- Barcode scanné
- SN normalisé
- Statut
- Raison
- Type matériel
- Date dernier contrôle
- Âge (jours)
- Résultat contrôle
- Scan manuel
- Notes SN
- Notes globales

---

## 📂 Nom du fichier

Format :

<Destination>_<Client>_<BR>_<YYYY-MM-DD>.csv

Exemples :

Retour_Semeru_BR-123_2026-05-18.csv  
Expedition_Veolia_BL-456_2026-05-18.csv

---

## 📤 Partage mobile

- Utilise navigator.share (API native)
- Compatible :
  - iOS (AirDrop, Mail, Teams…)
  - Android (partage natif)
- fallback automatique → téléchargement

---

## 📸 Scan terrain optimisé

- overlay de visée (cadre scan)
- ligne laser
- affichage résultat plein écran
- torche activable (si supportée)
- signal sonore :
  - OK → bip court
  - NOK → double bip

---

## 💾 Stockage

- localStorage uniquement
- aucune base externe
- persistance des scans
- mémorisation de la destination sélectionnée

---

## ⚠️ Contraintes

### Caméra

Fonctionne uniquement en :
- HTTPS ✅ (GitHub Pages recommandé)
- localhost ✅

Ne fonctionne pas en HTTP standard.

---

## 📦 Déploiement

### GitHub Pages

1. Créer un repository
2. Ajouter le fichier :

index.html

3. Activer GitHub Pages
4. Accès via :

https://<user>.github.io/<repo>

---

## 🔄 Workflow terrain

1. Charger fichier Excel de contrôle
2. Choisir destination
3. Scanner matériel
4. Ajouter notes si nécessaire
5. Valider via overlay
6. Exporter ou partager

---

## 🔌 Intégration Power Automate

Utilisations possibles :

- tri automatique par destination
- stockage SharePoint
- génération automatique PDF certificat
- alertes équipements non conformes
- création historique par client

---

## ⚙️ Détails techniques

### Librairies utilisées

- Quagga2 → scan code-barres
- SheetJS (xlsx) → lecture Excel

### Normalisation SN

- suppression du préfixe "V"
- suppression espaces
- conversion en majuscules

### Lecture Excel

- compatible multi-feuilles
- détection automatique des colonnes
- conservation du dernier contrôle par SN

---

## 🧪 Sécurité / UX

- destination obligatoire avant export
- confirmation avant suppression base
- overlay validation scan
- bouton purge isolé (évite erreurs terrain)

---

## 🛠️ Améliorations possibles

- upload automatique SharePoint
- ajout photo par scan
- génération PDF directe
- scan QR code
- API backend
- gestion multi-utilisateurs

---

## 👤 Usage cible

- gestion parc matériel
- location instrumentation
- maintenance terrain
- contrôle équipements

---

## ✅ Résultat

Application permettant :

✔ Scan rapide terrain  
✔ Vérification conformité automatique  
✔ Export structuré standardisé  
✔ Intégration directe workflow métier  

---

## 📄 Licence

Usage interne / professionnel
