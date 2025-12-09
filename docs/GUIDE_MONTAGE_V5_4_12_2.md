# 🔧 Guide de Montage — RPM Adapter Circuit

**Version circuit:** V5.4.12.2  
**Version guide:** V1.0  
**Date:** Décembre 2025  
**Statut:** ✅ VALIDÉ

---

## 📋 Avant de commencer

### Prérequis

- [ ] BOM complète (voir [BOM_V5_4_12_3.md](../bom/BOM_V5_4_12_3.md))
- [ ] Circuit imprimé (PCB) propre et inspecté
- [ ] Outils de soudure (station 350°C, étain 60/40)
- [ ] Multimètre numérique
- [ ] Pince à sertir embouts ferrule
- [ ] Tournevis plat 2,5mm pour borniers

### ⚠️ Avertissements de sécurité

> **HAUTE TENSION** — Ce circuit manipule des signaux jusqu'à -450V  
> **AUTOMOBILE** — Installation par personne qualifiée uniquement  
> **D1 CRITIQUE** — Une inversion de polarité détruira le circuit

---

## 🛠️ Étapes de montage

### Étape 1 : Préparation

#### 1.1 Inspection PCB
- [ ] Vérifier l'absence de court-circuits entre pistes
- [ ] Vérifier l'absence de pistes coupées
- [ ] Nettoyer le PCB (alcool isopropylique si nécessaire)

#### 1.2 Organisation composants
- [ ] Disposer tous les composants selon la BOM
- [ ] Identifier et marquer **D1 (1N5408)** avec une étiquette rouge
- [ ] Vérifier la polarité des condensateurs électrolytiques
- [ ] Vérifier l'orientation des diodes (repère cathode)

---

### Étape 2 : Soudure des résistances et composants passifs bas profil

#### 2.1 Résistances standard (R3, R4, R5)
- [ ] Souder R3 (680Ω, 0,25W)
- [ ] Souder R4 (470Ω, 0,25W)
- [ ] Souder R5 (6,8kΩ, 0,25W)
- [ ] Couper les pattes affleurantes

#### 2.2 Condensateurs céramiques (C3, C4, C6, C7, C_pré)
- [ ] Souder C3, C4, C6, C_pré (100nF, 50V)
- [ ] Souder C7 (15nF, 50V)

#### 2.3 Diodes signal (D6)
- [ ] Identifier la cathode de D6 (1N4148) — bague noire
- [ ] Souder D6 en respectant la polarité

---

### Étape 3 : Soudure des composants de puissance

#### 3.1 🔴 **DIODE CRITIQUE D1 (1N5408) — TRIPLE VÉRIFICATION**

> ⚠️ **PROCÉDURE OBLIGATOIRE** — Une erreur détruit le circuit

**Vérification #1** : Avant soudure
- [ ] Identifier la cathode de D1 (bague argentée sur le corps)
- [ ] Placer D1 sur le PCB : cathode vers le **régulateur U1** (downstream)
- [ ] Vérifier avec le schéma : cathode = côté positif après protection
- [ ] **Prendre une photo** de D1 positionné avant soudure

**Vérification #2** : Après soudure
- [ ] Inspecter visuellement la soudure de D1
- [ ] Vérifier au multimètre (mode diode) :
  - **Sens passant** (anode → cathode) : 0,6-0,7V
  - **Sens bloqué** (cathode → anode) : OL (ouvert)
- [ ] **Si les lectures sont inversées : DESSOUDER IMMÉDIATEMENT**

**Vérification #3** : Avant mise sous tension
- [ ] Re-vérifier visuellement l'orientation de D1
- [ ] Comparer avec la photo prise en vérification #1
- [ ] Demander une vérification par une seconde personne si disponible

#### 3.2 Autres diodes de puissance
- [ ] Souder D_flyback (1N5822) — respecter polarité
- [ ] Souder D3 (ZY27, Zener 27V) — respecter polarité
- [ ] Souder D5 (1N4733A, Zener 5,1V) — respecter polarité

#### 3.3 Résistances haute puissance (R1, R2)
- [ ] Souder R1 (47kΩ, 5W MOX) — surélever de 3-5mm du PCB
- [ ] Souder R2 (1kΩ, 3W MOX) — surélever de 2-3mm du PCB
- [ ] **Appliquer RTV silicone** : point de colle sur le corps de R1 (fixation anti-vibration)
- [ ] Laisser sécher 24h avant manipulation

---

### Étape 4 : Soudure des protections et condensateurs

#### 4.1 TVS et protections
- [ ] Souder D2 (15KPA18CA, TVS 18V) — respecter polarité
- [ ] **Appliquer RTV silicone** : point de colle sur le corps de D2 (fixation anti-vibration)
- [ ] Souder D4 (1.5KE250CA, TVS 250V) — respecter polarité
- [ ] Souder D7 (SA5.0CA, TVS 5V) — respecter polarité
- [ ] Laisser sécher RTV 24h

#### 4.2 Condensateurs électrolytiques 105°C
- [ ] **Vérifier température nominale : UNIQUEMENT 105°C automotive**
- [ ] Souder C1 (22µF, 50V, 105°C) — respecter polarité (patte longue = +)
- [ ] Souder C2 (22µF, 50V, 105°C) — respecter polarité
- [ ] Souder C5 (22µF, 16V, 105°C) — respecter polarité
- [ ] Vérifier visuellement : pas de condensateurs bombés

#### 4.3 Protections série
- [ ] Souder F1 (fusible 500mA fast-blow) avec support
- [ ] Souder PTC (RUEF050, 500mA)
- [ ] Souder L1 (inductance 10-22µH)

---

### Étape 5 : Soudure des circuits intégrés et connecteurs

#### 5.1 Régulateur et optocoupleur
- [ ] Souder U1 (R-78E5.0-0.5) — vérifier pinout VIN/GND/VOUT
- [ ] Souder U2 (H11L1M) — respecter orientation (repère point)

#### 5.2 Ferrites
- [ ] Préparer FB1 et FB2 (2 tours de fil sur anneau ferrite)
- [ ] Installer FB1 sur l'entrée bobine
- [ ] Installer FB2 sur l'entrée batterie

#### 5.3 Borniers avec embouts ferrule
- [ ] **Préparer les fils** :
  - Couper 6 fils (2 × J1, 2 × J2, 2 × J3)
  - Dénuder 6-7mm
  - **Sertir embouts ferrule** AWG 22-18 sur chaque fil
- [ ] Souder les borniers J1, J2, J3 sur le PCB
- [ ] **Appliquer frein-filet** Loctite 243 :
  - 1 goutte sur chaque vis de bornier
  - Attendre 10 minutes
  - Insérer les fils avec embouts ferrule
  - Serrer à 0,5-0,8 N⋅m (serrage modéré)

---

### Étape 6 : Finalisation et inspection

#### 6.1 Inspection visuelle complète
- [ ] Vérifier toutes les soudures (pas de ponts, pas de soudures froides)
- [ ] Vérifier l'absence de résidus de flux
- [ ] Vérifier la fixation mécanique (RTV sur D2 et R1 sec)
- [ ] Vérifier le serrage des borniers (embouts ferrule bien fixés)

#### 6.2 Nettoyage
- [ ] Nettoyer le PCB (alcool isopropylique + brosse douce)
- [ ] Sécher à l'air comprimé
- [ ] Inspection finale : pas de résidus

---

## 🔬 Tests de validation

### Tests statiques (hors tension)

#### Test 1 : Continuité alimentation
- [ ] Multimètre en mode continuité
- [ ] Vérifier court-circuit entre J1+ et J1− : **OL (ouvert)**
- [ ] Vérifier continuité J1+ → D1 anode : **0Ω**
- [ ] Vérifier isolation J1+ → GND : **OL (ouvert)**

#### Test 2 : Isolation galvanique
- [ ] Vérifier isolation entrée bobine J2 ↔ sortie J3 : **OL (ouvert)**
- [ ] Vérifier isolation alimentation J1 ↔ sortie J3 : **OL (ouvert)**

#### Test 3 : Polarité D1 (dernière vérification)
- [ ] Multimètre en mode diode
- [ ] D1 sens passant (J1+ → cathode D1) : **0,6-0,7V**
- [ ] D1 sens bloqué (cathode D1 → J1+) : **OL**
- [ ] **Si inversé : ARRÊT — Dessouder D1**

### Tests dynamiques (sous tension)

> ⚠️ **ATTENTION** : Tests sous 12V — Respecter les polarités

#### Test 4 : Alimentation 12V
- [ ] **VÉRIFICATION FINALE D1** : orientation cathode confirmée ×3
- [ ] Connecter alimentation 12V stabilisée (limitée à 1A)
- [ ] Mesurer tension à J1+ : **12,0V ± 0,5V**
- [ ] Mesurer tension après D1 (cathode) : **11,3-11,4V** (chute 0,6-0,7V)
- [ ] Mesurer tension sortie U1 : **5,0V ± 0,1V**

#### Test 5 : Consommation à vide
- [ ] Mesurer courant d'alimentation : **< 50mA**
- [ ] Vérifier température U1 après 5 min : **< 40°C** (tiède)

#### Test 6 : Signal de sortie (sans entrée bobine)
- [ ] Mesurer tension J3 (sortie VTACH) : **5,0V ± 0,2V** (état repos)

---

## 🧪 Tests sur véhicule

### Préparation installation

- [ ] Circuit validé en labo (tests statiques + dynamiques OK)
- [ ] Prévoir fusible 1A supplémentaire sur l'alimentation 12V véhicule
- [ ] Prévoir connecteur GX12-5 pour bobine (si applicable)

### Installation

1. **Alimentation (J1)** :
   - [ ] J1+ sur 12V post-contact (fusible 1A en série)
   - [ ] J1− sur masse châssis propre

2. **Entrée bobine (J2)** :
   - [ ] Connecter J2 sur signal bobine cylindre #1
   - [ ] Vérifier polarité : J2+ = sortie bobine, J2− = masse

3. **Sortie VTACH (J3)** :
   - [ ] J3+ vers entrée VTACH Innovate MTX-D
   - [ ] J3− vers masse Innovate MTX-D

### Tests dynamiques véhicule

#### Test 7 : RPM ralenti
- [ ] Démarrer le moteur
- [ ] Vérifier affichage RPM sur Innovate : **~700 RPM** (±50 RPM)
- [ ] Vérifier stabilité (pas de fluctuations)

#### Test 8 : RPM accélération
- [ ] Accélérer progressivement jusqu'à 3000 RPM
- [ ] Vérifier suivi linéaire (pas de décrochage)
- [ ] Vérifier absence de double comptage (RPM stable)

#### Test 9 : Température R1 (30 minutes)
- [ ] Rouler 30 minutes en conditions normales
- [ ] Mesurer température R1 (thermomètre IR) : **< 60°C**
- [ ] Vérifier température U1 : **< 50°C**

---

## 🔧 Dépannage

### Problème : Pas de 5V en sortie U1

**Causes possibles** :
- D1 inversé → Vérifier polarité D1 (test diode)
- Fusible F1 grillé → Remplacer F1 (vérifier cause : court-circuit ?)
- Court-circuit sur rail 5V → Vérifier isolation U1/U2

**Actions** :
1. Vérifier tension après D1 : doit être 11,3-11,4V
2. Si 0V après D1 : **D1 inversé** — Dessouder et corriger
3. Si 11,3V après D1 mais 0V sur U1 : Vérifier F1, PTC, L1

### Problème : Pas de signal RPM sur Innovate

**Causes possibles** :
- Pas de signal d'entrée bobine → Vérifier connexion J2
- U2 (H11L1M) défaillant → Vérifier LED interne U2
- Sortie J3 en court-circuit → Vérifier isolation J3

**Actions** :
1. Mesurer tension J2 au ralenti : doit pulsée (0-12V)
2. Mesurer tension J3 au ralenti : doit pulser (0-5V)
3. Si J2 OK mais pas J3 : Vérifier U2, R3, D5

### Problème : Double comptage RPM

**Causes possibles** :
- Rebonds signal d'entrée → Vérifier C7 (15nF)
- Ferrites FB1 insuffisantes → Ajouter 1 tour supplémentaire

**Actions** :
1. Vérifier présence C7 (15nF) sur entrée U2
2. Vérifier FB1 : doit avoir 2 tours minimum
3. Augmenter C7 à 22nF si nécessaire (test)

### Problème : R1 surchauffe (> 60°C)

**Causes possibles** :
- Signal bobine trop élevé (> -450V) → Vérifier bobine
- R1 sous-dimensionné → Vérifier R1 = 47kΩ 5W

**Actions** :
1. Mesurer tension bobine (oscilloscope) : pic doit être < -450V
2. Vérifier R1 : 47kΩ ± 5%, 5W minimum
3. Si > -450V : Augmenter R1 à 68kΩ 5W (test)

---

## ✅ Checklist finale

### Avant mise sous tension

- [ ] D1 (1N5408) orientation vérifiée ×3 (cathode vers U1)
- [ ] Toutes les soudures inspectées et validées
- [ ] Tous les condensateurs 105°C (C1, C2, C5)
- [ ] RTV silicone sec (24h) sur D2 et R1
- [ ] Embouts ferrule sertis et insérés dans borniers
- [ ] Frein-filet appliqué sur vis borniers
- [ ] Tests statiques OK (continuité, isolation, polarité)

### Après mise sous tension (labo)

- [ ] Tension après D1 : 11,3-11,4V
- [ ] Tension sortie U1 : 5,0V ± 0,1V
- [ ] Consommation à vide : < 50mA
- [ ] Température U1 après 5 min : < 40°C
- [ ] Tension sortie J3 au repos : 5,0V ± 0,2V

### Après installation véhicule

- [ ] RPM ralenti affiché : ~700 RPM
- [ ] RPM accélération : suivi linéaire sans décrochage
- [ ] Absence de double comptage
- [ ] Température R1 après 30 min : < 60°C
- [ ] Pas de bruits parasites sur jauge
- [ ] Fixations mécaniques vérifiées (pas de jeu)

---

## 📚 Références

- [BOM V5.4.12.3](../bom/BOM_V5_4_12_3.md) — Liste complète des composants
- [Circuit V5.4.12.2](Circuit_RPM_V5_4_12_2.md) — Schéma détaillé
- [Protocole Test V2.7.3](PROTOCOLE_TEST_RPM_V2_7_3.md) — Tests complets

---

## ⚠️ Notes finales

> **D1 CRITIQUE** — La diode D1 (1N5408) est le composant le plus critique. Une inversion de polarité détruira le régulateur U1 et potentiellement d'autres composants. Vérifier ×3 avant mise sous tension.

> **CONDENSATEURS 105°C** — N'utiliser QUE des condensateurs 105°C automotive pour C1, C2, C5. Les condensateurs 85°C standard auront une durée de vie réduite (< 2 ans en environnement moteur).

> **FIXATIONS MÉCANIQUES** — Le RTV silicone, les embouts ferrule et le frein-filet sont essentiels pour la fiabilité long terme en environnement vibratoire. Ne pas négliger ces étapes.

---

**FIN GUIDE MONTAGE V5.4.12.2**
