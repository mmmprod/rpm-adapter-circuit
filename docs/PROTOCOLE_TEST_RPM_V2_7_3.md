# 🧪 Protocole de test RPM Adapter V2.7.3

**Version:** V2.7.3  
**Circuit:** RPM Adapter V5.4.12.2  
**Date:** Décembre 2025  
**Statut:** ✅ VALIDÉ

---

## 🎯 Objectif

Validation complète du circuit RPM Adapter avant installation sur véhicule Ford Mustang EcoBoost.

---

## 📋 Matériel requis

### Équipement de laboratoire

| Équipement | Spécifications | Usage |
|------------|----------------|-------|
| Alimentation DC | 0-15V, 2A min | Simulation batterie 12V |
| Multimètre | True RMS | Mesures tension/courant |
| Oscilloscope | 100MHz min, 2 voies | Analyse signaux |
| Générateur de fonctions | 0-100Hz, 0-10V | Simulation signal RPM |
| Thermomètre IR | -20°C à +200°C | Mesure température composants |
| Sonde de courant | 0-5A | Mesure consommation |

### Équipement terrain

| Équipement | Usage |
|------------|-------|
| Jauge Innovate MTX-D | Affichage RPM |
| OBD2 scanner | Référence RPM ECU |
| Câblage test | Connexion bobine |

---

## ⚡ PHASE 1 — Tests laboratoire (sans signal)

### TEST 1.1 — Inspection visuelle

**Objectif:** Vérifier l'assemblage et l'orientation des composants.

**Checklist:**
- [ ] D1 (1N5408) : Orientation cathode vers +12V ⚠️
- [ ] D2 (TVS) : Orientation correcte (bidirectionnel)
- [ ] D4 (TVS) : Installé
- [ ] D5 (Zener) : Cathode vers signal
- [ ] C1, C2, C5 : Polarité correcte (électrolytiques)
- [ ] U1 (R-78E) : Orientation selon sérigraphie
- [ ] U2 (H11L1M) : Broche 1 repérée
- [ ] Soudures : Pas de ponts, aspect brillant
- [ ] R1 (47kΩ) : Marquage MOX visible

**Critère GO:** Tous les points vérifiés ✅

---

### TEST 1.2 — Test de continuité (hors tension)

**Objectif:** Vérifier l'absence de courts-circuits.

**Procédure:**
1. Multimètre en mode continuité
2. Mesurer entre +12V et GND : **OUVERT** (pas de bip)
3. Mesurer entre +5V et GND : **OUVERT**
4. Mesurer entre J2 et GND : **Résistance élevée** (>40kΩ via R1)

**Critères GO:**
- Pas de court-circuit +12V/GND ✅
- Pas de court-circuit +5V/GND ✅
- R1 détectable côté bobine ✅

---

### TEST 1.3 — Première mise sous tension (12V)

**Objectif:** Valider l'alimentation et D1.

**Procédure:**
1. Alimentation DC réglée sur **12,0V**
2. Limiter courant à **1A**
3. Connecter J1 (respecter polarité)
4. **Mesurer tension après D1** (TP1)
5. Mesurer sortie U1 (TP2, +5V)
6. Mesurer courant consommé

**Valeurs attendues:**
| Point | Valeur | Tolérance |
|-------|--------|-----------|
| TP1 (après D1) | 11,3-11,5V | ±0,2V |
| TP2 (sortie U1) | 5,00V | ±0,1V |
| Courant total | 50-100mA | — |

**Critères GO:**
- Tension TP1 : 11,3-11,5V ✅
- Tension TP2 : 5,0V ±2% ✅
- Pas de composant chaud (>60°C) ✅

**Critères NO-GO:**
- ⛔ Tension TP1 < 11V → Vérifier D1
- ⛔ Tension TP2 ≠ 5V → Vérifier U1, D3, condensateurs
- ⛔ Courant > 200mA → Court-circuit

---

### TEST 1.4 — Test de charge alimentation

**Objectif:** Valider la capacité de l'alimentation sous charge.

**Procédure:**
1. Ajouter charge résistive 100Ω sur +5V (50mA)
2. Mesurer stabilité +5V
3. Augmenter charge à 47Ω (100mA)
4. Mesurer température U1 après 5 minutes

**Valeurs attendues:**
- +5V stable ±1% sous charge 100mA
- Température U1 < 50°C

**Critères GO:**
- Régulation stable ✅
- Pas de surchauffe ✅

---

## 🔊 PHASE 2 — Tests avec signal simulé

### TEST 2.1 — Signal basse fréquence (10Hz, ralenti simulé)

**Objectif:** Valider la chaîne de traitement signal.

**Procédure:**
1. Générateur : **10Hz, 5Vpp, offset 0V** (0-5V)
2. Injecter sur J2 (entrée bobine)
3. Oscilloscope voie 1 : Entrée J2
4. Oscilloscope voie 2 : Sortie VTACH (J3)
5. Vérifier rapport de forme

**Valeurs attendues:**
- Fréquence sortie = Fréquence entrée (10Hz)
- Signal sortie : 0-5V logique propre
- Temps de montée/descente < 1µs
- Pas de rebonds

**Critères GO:**
- Fréquence identique ✅
- Niveaux logiques propres ✅
- Pas de double comptage ✅

---

### TEST 2.2 — Signal moyenne fréquence (50Hz, 3000 RPM)

**Procédure:**
1. Générateur : **50Hz, 5Vpp**
2. Vérifier sortie VTACH
3. Mesurer température R1 après 10 minutes

**Valeurs attendues:**
- Signal stable 50Hz
- Température R1 < 40°C (charge faible)

**Critères GO:**
- Signal stable ✅
- Pas de surchauffe ✅

---

### TEST 2.3 — Signal haute fréquence (100Hz, 6000 RPM)

**Procédure:**
1. Générateur : **100Hz, 5Vpp**
2. Vérifier suivi de fréquence
3. Mesurer latence

**Valeurs attendues:**
- Fréquence sortie = 100Hz
- Latence < 10µs

**Critères GO:**
- Suivi parfait haute fréquence ✅

---

### TEST 2.4 — Test température R1 (charge simulée)

**Objectif:** Valider dissipation thermique R1.

**Procédure:**
1. Générateur : **10Hz, 10Vpp** (amplitude double)
2. Laisser fonctionner **30 minutes**
3. Mesurer température R1 toutes les 5 minutes

**Valeurs attendues:**
| Temps | T_max R1 |
|-------|----------|
| 5 min | < 45°C |
| 10 min | < 50°C |
| 15 min | < 55°C |
| 30 min | < 60°C |

**Critères GO:**
- Température R1 < 60°C après 30 min ✅
- Stabilisation thermique atteinte ✅

**Critères NO-GO:**
- ⛔ T_R1 > 60°C → Vérifier dissipateur ou réduire charge

---

## 🚗 PHASE 3 — Tests sur véhicule (Ford Mustang)

### TEST 3.1 — Installation et câblage

**Procédure:**
1. Couper contact véhicule
2. Débrancher bobine cylindre 1
3. Connecter J1 : +12V batterie (via fusible)
4. Connecter J2 : Signal bobine
5. Connecter J3 : Entrée VTACH Innovate MTX-D
6. Vérifier tous les serrages

**Checklist:**
- [ ] Fusible 500mA installé sur +12V
- [ ] Masse commune correcte
- [ ] Pas de câbles près de sources de chaleur
- [ ] Protection mécanique OK
- [ ] MTX-D configuré mode VTACH

---

### TEST 3.2 — Test moteur tournant (ralenti)

**Objectif:** Valider affichage RPM ralenti.

**Procédure:**
1. Démarrer moteur
2. Laisser stabiliser ralenti (~700 RPM)
3. Comparer RPM MTX-D vs OBD2

**Valeurs attendues:**
| Source | RPM |
|--------|-----|
| OBD2 | 700 ±20 RPM |
| MTX-D | 700 ±20 RPM |
| Écart | < 5% |

**Critères GO:**
- Affichage stable ✅
- Écart < 5% ✅
- Pas de clignotement ✅

---

### TEST 3.3 — Test accélération progressive

**Objectif:** Vérifier suivi linéaire RPM.

**Procédure:**
1. Accélérer progressivement 700 → 3000 RPM
2. Observer suivi MTX-D
3. Vérifier absence de décrochage

**Critères GO:**
- Suivi linéaire sans sauts ✅
- Pas de double comptage ✅
- Réactivité < 0,5s ✅

---

### TEST 3.4 — Test haute charge (montée en régime rapide)

**Objectif:** Valider comportement transitoires rapides.

**Procédure:**
1. Accélération franche 700 → 5000 RPM
2. Observer stabilité affichage
3. Vérifier température R1 après 5 minutes

**Critères GO:**
- Pas de perte signal ✅
- Affichage stable ✅
- T_R1 < 60°C ✅

---

### TEST 3.5 — Test longue durée (30 minutes)

**Objectif:** Valider fiabilité continue.

**Procédure:**
1. Rouler 30 minutes (conditions mixtes)
2. Vérifier stabilité affichage RPM
3. Mesurer température R1 et U1 à l'arrêt

**Valeurs attendues:**
- Affichage stable pendant 30 min
- T_R1 < 60°C
- T_U1 < 50°C

**Critères GO:**
- Fonctionnement stable 30 min ✅
- Pas de surchauffe ✅

---

## 🔍 PHASE 4 — Tests de robustesse (optionnels)

### TEST 4.1 — Test transitoires batterie

**Procédure:**
1. Simuler load-dump : +18V pendant 100ms
2. Vérifier protection D2
3. Vérifier +5V stable

**Critères GO:**
- Circuit survit +18V ✅
- +5V régulé stable ✅

---

### TEST 4.2 — Test inversion polarité (destructif)

⚠️ **Test DANGEREUX — Ne PAS effectuer sur circuit final**

**Objectif:** Valider protection D1.

**Procédure (sur circuit de test uniquement):**
1. Inverser +12V et GND
2. Vérifier pas de courant
3. Rétablir polarité correcte
4. Vérifier fonctionnement normal

**Critères GO:**
- Pas de destruction composants ✅
- Fonctionnement normal après correction ✅

---

## 📊 Checklist finale GO / NO-GO

### Critères GO (tous requis)

- [ ] Alimentation +5V stable (±2%)
- [ ] RPM ralenti affiché correctement (~700 RPM)
- [ ] Suivi linéaire accélération
- [ ] Absence de double comptage
- [ ] Température R1 < 60°C (30 min)
- [ ] Chute tension D1 acceptable (0,6-0,7V)
- [ ] Affichage stable 30 minutes
- [ ] Pas de composants surchauffés

### Critères NO-GO (un seul suffit)

- ⛔ Tension +5V instable ou hors tolérance
- ⛔ RPM incorrect ou instable
- ⛔ Double comptage détecté
- ⛔ Température R1 > 60°C
- ⛔ Composant défaillant ou endommagé
- ⛔ Court-circuit détecté

---

## 📝 Rapport de test

### Template rapport

```
RAPPORT DE TEST RPM ADAPTER V5.4.12.2
Date : __________
Testeur : __________

PHASE 1 — Laboratoire
[ ] TEST 1.1 : Inspection visuelle — GO / NO-GO
[ ] TEST 1.2 : Continuité — GO / NO-GO
[ ] TEST 1.3 : Première mise sous tension — GO / NO-GO
    TP1 : ______ V
    TP2 : ______ V
    I_total : ______ mA
[ ] TEST 1.4 : Test de charge — GO / NO-GO

PHASE 2 — Signal simulé
[ ] TEST 2.1 : 10Hz — GO / NO-GO
[ ] TEST 2.2 : 50Hz — GO / NO-GO
[ ] TEST 2.3 : 100Hz — GO / NO-GO
[ ] TEST 2.4 : Température R1 (30 min) : ______ °C — GO / NO-GO

PHASE 3 — Véhicule
[ ] TEST 3.1 : Installation — GO / NO-GO
[ ] TEST 3.2 : Ralenti — GO / NO-GO
    OBD2 : ______ RPM
    MTX-D : ______ RPM
    Écart : ______ %
[ ] TEST 3.3 : Accélération — GO / NO-GO
[ ] TEST 3.4 : Haute charge — GO / NO-GO
[ ] TEST 3.5 : 30 minutes — GO / NO-GO

DÉCISION FINALE
[ ] GO — CIRCUIT VALIDÉ POUR PRODUCTION
[ ] NO-GO — CORRECTIONS REQUISES

Notes : _________________________________
```

---

## ⚠️ Consignes de sécurité

> **HAUTE TENSION** — Ne jamais toucher R1 ou D4 moteur tournant

> **BATTERIE** — Toujours couper contact avant modifications

> **FUSIBLE** — Remplacer uniquement par 500mA Fast-blow

> **TEMPÉRATURE** — Arrêter test si composant > 80°C

---

**FIN PROTOCOLE TEST V2.7.3**

**Statut : VALIDÉ** ✅
