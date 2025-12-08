# PROTOCOLE DE TEST RPM ADAPTER V2.7.3

**Version:** 2.7.3  
**Date:** 01 Décembre 2025  
**Circuit référence:** V5.4.12.1  
**Auteur:** Mehdi  
**Application:** Ford Mustang EcoBoost 2016 Stage 4+ → Innovate MTX-D  
**Conformité:** ✅ PREMORTEM V3.4 VALIDÉ  
**Statut:** ✅ PROTOCOLE VALIDÉ

---

## SECTION 0 — SÉCURITÉ

### ⚠️ AVERTISSEMENTS CRITIQUES

**HAUTE TENSION:**
- Le signal bobine atteint **-300V à -450V**
- Toujours couper le contact avant manipulation
- Ne jamais toucher les connexions bobine moteur en marche
- Porter des gants isolants classe 0 (500V) minimum

**RISQUE INCENDIE:**
- Circuit alimenté par batterie 12V (courant illimité)
- Fusible 500mA obligatoire côté batterie
- Vérifier l'absence de court-circuit avant mise sous tension
- Avoir un extincteur classe C à portée de main

**PROTECTION PERSONNELLE:**
- Lunettes de sécurité obligatoires
- Gants isolants recommandés
- Chaussures isolantes
- Environnement ventilé

### Prérequis qualification opérateur

- [ ] Formation électronique automobile
- [ ] Expérience circuits haute tension (>100V)
- [ ] Connaissance systèmes allumage moteur
- [ ] Lecture schémas électroniques

### Conditions d'environnement

| Paramètre | Valeur requise |
|-----------|----------------|
| Température ambiante | 15°C à 30°C |
| Humidité relative | < 70% |
| Éclairage | > 500 lux |
| Zone de travail | Propre, dégagée, non-conductrice |

---

## SECTION 1 — OUTILLAGE REQUIS

### Équipement de mesure

**Multimètre numérique (obligatoire):**
- Résolution: 0,1Ω (ohmmètre), 0,01V (voltmètre)
- Précision: ±0,5% minimum
- CAT III 600V minimum
- Exemple: Fluke 117, Brymen BM869s

**Oscilloscope (validation complète):**
- Bande passante: ≥100 MHz
- Fréquence échantillonnage: ≥500 MSa/s
- Sonde HT: 100:1, 40kV (signal bobine)
- Exemple: Rigol DS1054Z, Siglent SDS1104X-E

**Pyromètre infrarouge (tests charge):**
- Plage: -50°C à +300°C
- Précision: ±2°C
- Émissivité réglable

### Alimentation de test

**Source tension stabilisée:**
- Tension: 0-15V DC réglable
- Courant: ≥1A
- Limitation courant réglable
- Protection court-circuit
- Exemple: Tenma 72-2535, Velleman LABPS3005D

**Alternative — Batterie plomb 12V:**
- Capacité: ≥7Ah
- Tension: 12,6V à 13,8V
- État: Chargée, testée

### Câblage et connectique

- Câbles test silicone 14AWG rouge/noir (×4)
- Pinces crocodile isolées moyennes (×4)
- Câble BNC-BNC pour oscilloscope (×2)
- Sonde oscilloscope 10:1 standard (×1)
- Sonde oscilloscope 100:1 HT (×1, test bobine)

### Consommables

- Fusible verre 5×20mm 500mA Fast-blow (×5)
- Fusible lame ATO 1A (véhicule) (×2)
- Ruban isolant PVC noir
- Serre-câbles 100mm (×10)
- Chiffon microfibre non-pelucheux

### Documentation

- [ ] Schéma circuit V5.4.12.1
- [ ] BOM complète
- [ ] Ce protocole de test (imprimé)
- [ ] Datasheet R-78E5.0-0.5
- [ ] Datasheet H11L1M

---

## SECTION 2 — INSPECTION VISUELLE

### Avant mise sous tension (CRITIQUE)

**2.1 — Composants soudés:**
- [ ] Tous les composants présents selon BOM
- [ ] Polarité correcte: D1, D_flyback, D2, D3, D4, D5, D6, D7
- [ ] Polarité correcte: C1, C2, C5 (électrolytiques)
- [ ] Orientation U1 (R-78E): pin1 VIN, pin2 GND, pin3 VOUT
- [ ] Orientation U2 (H11L1): repère pin1 visible
- [ ] Soudures: brillantes, pas de pont ni soudure froide

**2.2 — Pistes et connecteurs:**
- [ ] Aucune trace de surchauffe sur PCB
- [ ] Pas de coupure visible pistes cuivre
- [ ] Pas de résidu flux conducteur
- [ ] Borniers J1, J2, J3 vissés correctement
- [ ] Trous de vis PCB dégagés, filetage OK

**2.3 — Composants critiques haute puissance:**
- [ ] R1 (47kΩ 5W): corps céramique intact, pattes 2mm min
- [ ] R2 (1kΩ 3W): corps céramique intact
- [ ] D4 (1.5KE250CA): corps DO-201AE, bague visible
- [ ] L1 (inductance blindée): boîtier fermé, pas d'impact

**2.4 — Test ohmmètre court-circuits (alim OFF):**
- [ ] Résistance +12V → GND: > 100kΩ (protection en place)
- [ ] Résistance +5V → GND: > 10kΩ (H11L1 sortie inactive)
- [ ] Résistance VTACH → GND: > 5kΩ (pull-up R5 présent)

**DÉCISION:** Si 1 seul ✗ → ARRÊT test, correction obligatoire

---

## SECTION 2BIS — FUSIBLE LAME 1A VÉHICULE

### Contexte

Le circuit possède un fusible intégré **F1 (500mA)** protégeant le circuit lui-même. Pour protéger le véhicule, un **fusible lame ATO 1A** doit être ajouté **côté batterie** avant le circuit.

### Implémentation

**Porte-fusible lame inline:**
- Type: ATO/ATC inline, étanche IP67
- Câble: 14AWG minimum
- Exemple: Littelfuse 0297001.WXNV, Bussman BP/HHH

**Installation:**
```
[Batterie +12V] → [Fusible lame 1A] → [vers circuit J1 pin1]
```

**Position:**
- À moins de 30cm de la borne batterie
- Fixé mécaniquement (pas suspendu)
- Câblage protégé gaine tressée

**Tests:**
- [ ] Fusible 1A installé côté batterie
- [ ] Continuité +BATT → porte-fusible → J1 pin1
- [ ] Résistance totale câble: < 0,5Ω
- [ ] Porte-fusible étanche, propre, clippage ferme

---

## SECTION 3 — TESTS MULTIMÈTRE (SANS ALIMENTATION)

### 3.1 — Résistances critiques

**R1 (47kΩ 5W):**
- Mesuré: _______ kΩ
- Tolérance: 46,5 kΩ à 47,5 kΩ (±1%)
- GO: ✅ / NO-GO: ❌

**R2 (1kΩ 3W):**
- Mesuré: _______ Ω
- Tolérance: 980Ω à 1020Ω (±2%)
- GO: ✅ / NO-GO: ❌

**R3 (680Ω):**
- Mesuré: _______ Ω
- Tolérance: 646Ω à 714Ω (±5%)
- GO: ✅ / NO-GO: ❌

### 3.2 — Diodes (test diode multimètre)

**D1, D_flyback (1N5822 Schottky):**
- Vf forward (sens passant): 0,25V à 0,35V
- Sens bloqué: OL (circuit ouvert)
- D1 mesuré: _______ V, GO: ✅ / NO-GO: ❌
- D_flyback mesuré: _______ V, GO: ✅ / NO-GO: ❌

**D5 (1N4733A Zener 5,1V):**
- Sens forward: 0,6V à 0,8V
- Sens inverse (Zener): Non testable multimètre
- Mesuré forward: _______ V, GO: ✅ / NO-GO: ❌

**D6 (1N4148):**
- Vf forward: 0,6V à 0,8V
- Sens bloqué: OL
- Mesuré: _______ V, GO: ✅ / NO-GO: ❌

### 3.3 — Condensateurs (capacimètre)

**C1, C2 (22µF 50V électrolytiques):**
- Tolérance: ±20% (17,6µF à 26,4µF)
- C1 mesuré: _______ µF, GO: ✅ / NO-GO: ❌
- C2 mesuré: _______ µF, GO: ✅ / NO-GO: ❌

**C5 (22µF 16V électrolytique):**
- Tolérance: ±20%
- Mesuré: _______ µF, GO: ✅ / NO-GO: ❌

---

## SECTION 4 — TEST ALIMENTATION 12V (SANS CHARGE)

### Configuration

**Alimentation stabilisée:**
- Tension: 13,8V DC (simule moteur tournant)
- Limitation courant: 500mA
- Fusible F1 installé dans circuit

**Connexions:**
- Alim (+) → J1 pin1 (+12V)
- Alim (−) → J1 pin2 (GND)
- Multimètre: mesure tension +12V_PROT_PWR → GND

### Procédure

1. Régler alim 13,8V, limiter 500mA, OUTPUT OFF
2. Connecter câbles (vérifier polarité ×3)
3. Multimètre en mode VDC, calibre 20V
4. OUTPUT ON
5. Observer appel courant initial (charge condensateurs)

### Résultats attendus

**Tension +12V_PROT_PWR:**
- Attendu: 13,3V à 13,7V (chute 0,1V à 0,5V via D1)
- Mesuré: _______ V
- GO: ✅ / NO-GO: ❌

**Courant consommé (sans charge):**
- Attendu: < 5mA (fuite diélectrique uniquement)
- Mesuré: _______ mA
- GO: ✅ / NO-GO: ❌

**Température fusible F1 (après 1min):**
- Attendu: Ambiant +2°C max
- Mesuré: _______ °C
- GO: ✅ / NO-GO: ❌

**DÉCISION:** Si 1 seul NO-GO → ARRÊT, diagnostic Section Annexe A

---

## SECTION 5 — TEST RÉGULATEUR 5V

### Configuration (suite Section 4)

Alimentation 12V maintenue, ajouter mesure +5V.

### Procédure

1. Multimètre en VDC, calibre 10V
2. Mesurer +5V (sortie U1 pin3) → GND
3. Attendre stabilisation 10 secondes
4. Noter tension +5V

### Résultats attendus

**Tension +5V:**
- Attendu: 4,95V à 5,05V (±1%)
- Mesuré: _______ V
- GO: ✅ / NO-GO: ❌

**Courant total circuit (alim 12V):**
- Attendu: 5mA à 15mA (R-78E + H11L1 repos)
- Mesuré: _______ mA
- GO: ✅ / NO-GO: ❌

**Température U1 (R-78E) après 2min:**
- Attendu: Ambiant +5°C à +15°C
- Mesuré: _______ °C
- GO: ✅ / NO-GO: ❌

---

## SECTION 6 — TEST SORTIE VTACH (SANS SIGNAL BOBINE)

### Configuration

Circuit alimenté 12V, +5V stable.

### Procédure

1. Mesurer tension VTACH (J3 pin1) → GND
2. Vérifier état logique haut (H11L1 ouvert au repos)

### Résultats attendus

**Tension VTACH:**
- Attendu: 4,5V à 5,0V (pull-up R5 vers +5V)
- Mesuré: _______ V
- GO: ✅ / NO-GO: ❌

**Interprétation:**
- Si < 4,5V → H11L1 conduit (anomalie) ou R5 défaut
- Si > 5,0V → Impossible (rail +5V)

---

## SECTION 7 — TEST INJECTION SIGNAL SIMULÉ BOBINE

### Configuration

**Générateur de signaux:**
- Forme: Impulsions rectangulaires négatives
- Amplitude: -5V (sécurité test)
- Fréquence: 50Hz (3000 RPM moteur 4 cylindres)
- Rapport cyclique: 10% (pulse 2ms)

**Connexions:**
- Générateur COM → GND circuit
- Générateur OUT → J2 pin1 (SIGNAL)
- Oscilloscope CH1: SIGNAL bobine (J2 pin1)
- Oscilloscope CH2: VTACH (J3 pin1)

### Procédure

1. Régler générateur -5V, 50Hz, 10% duty
2. Connecter générateur → J2 pin1
3. OUTPUT ON générateur
4. Observer VTACH oscilloscope CH2

### Résultats attendus

**Signal VTACH:**
- Forme: Impulsions 0V → 5V (logique inversée)
- Fréquence: 50Hz (synchrone avec entrée)
- Amplitude: 4,8V à 5,0V (haut), < 0,5V (bas)
- Temps montée: < 10µs
- Temps descente: < 10µs

**Mesures:**
- Fréquence VTACH: _______ Hz, GO: ✅ / NO-GO: ❌
- Niveau haut: _______ V, GO: ✅ / NO-GO: ❌
- Niveau bas: _______ V, GO: ✅ / NO-GO: ❌

**Température R1 (après 5min test):**
- Attendu: Ambiant +2°C (signal faible -5V)
- Mesuré: _______ °C, GO: ✅ / NO-GO: ❌

---

## SECTION 8 — TEST PROTECTION HAUTE TENSION (OPTIONNEL)

### ⚠️ DANGER — Réservé personnel qualifié HT

**Configuration:**
- Générateur HT capable -100V à -300V
- Sonde oscilloscope 100:1 (40kV)
- Limiteur courant série 100kΩ 10W (sécurité)

**Procédure:**
1. Circuit alimenté 12V
2. Générateur HT: -100V, 50Hz, pulse 2ms
3. Injecter sur J2 pin1 via limiteur 100kΩ
4. Observer VTACH oscilloscope
5. Augmenter par paliers -50V: -100V, -150V, -200V, -250V

**Résultats attendus:**
- VTACH stable 50Hz sur toute plage -100V à -250V
- Température R1: < 60°C à -250V après 10min
- Aucune fumée, odeur, LED H11L1 visible normale

**ARRÊT IMMÉDIAT si:**
- Fumée visible
- Température R1 > 80°C
- VTACH instable ou absent
- Odeur composant brûlé

---

## SECTION 9 — TEST CHARGE THERMIQUE R1

### Configuration

Circuit alimenté, signal bobine simulé -5V, 50Hz continu.

### Procédure

1. Laisser tourner 30 minutes
2. Mesurer température R1 chaque 5 minutes
3. Noter température max atteinte

**Tableau relevés:**

| Temps (min) | Temp R1 (°C) |
|-------------|--------------|
| 0 | ______ |
| 5 | ______ |
| 10 | ______ |
| 15 | ______ |
| 20 | ______ |
| 25 | ______ |
| 30 | ______ |

### Résultats attendus

**Température max R1:**
- Attendu: < 50°C (marge 200°C sur Tmax 250°C)
- Mesuré: _______ °C
- GO: ✅ / NO-GO: ❌

**Stabilité VTACH (contrôle oscilloscope):**
- Fréquence stable: GO: ✅ / NO-GO: ❌
- Pas de dérive amplitude: GO: ✅ / NO-GO: ❌

---

## SECTION 10 — TEST IMMUNITÉ PERTURBATIONS

### 10.1 — Transitoires alimentation 12V

**Procédure:**
- Circuit alimenté, VTACH stable
- Débrancher/rebrancher alimentation 12V rapidement (×5)
- Observer VTACH oscilloscope

**Résultat attendu:**
- VTACH reprend < 100ms après reconnexion
- Aucun dommage circuit

### 10.2 — Décharge électrostatique (ESD)

**Procédure:**
- Générateur ESD: décharge 4kV sur boîtier métallique
- Observer VTACH

**Résultat attendu:**
- VTACH inchangé ou reprend < 1s
- Aucun plantage H11L1

---

## SECTION 11 — VALIDATION VÉHICULE TERRAIN

### Prérequis

- Tous les tests banc Section 4 à 10 GO
- Installation physique circuit terminée
- Câblage véhicule selon schéma
- Fusible lame 1A côté batterie installé

### Configuration véhicule

**Connexions:**
- J1: Batterie 12V via fusible lame 1A
- J2: Fil bobine ignition cylindre #1 (signal −)
- J3: Innovate MTX-D entrée RPM

**Matériel:**
- Multimètre sur VTACH (J3 pin1)
- Innovate MTX-D en mode affichage RPM

### Procédure

**11.1 — Contact ON, moteur OFF:**
- [ ] Tension VTACH: 4,5V à 5,0V (niveau haut)
- [ ] Innovate affiche: 0 RPM ou "---"

**11.2 — Démarrage moteur (ralenti ~700 RPM):**
- [ ] VTACH pulse visible multimètre (clignotement)
- [ ] Innovate affiche: 650 RPM à 750 RPM
- [ ] Affichage stable (pas de double comptage)

**11.3 — Montée progressive RPM (accélération modérée):**
- [ ] 1500 RPM: Innovate affiche 1450-1550 RPM
- [ ] 3000 RPM: Innovate affiche 2950-3050 RPM
- [ ] 4500 RPM: Innovate affiche 4450-4550 RPM
- [ ] Aucun décrochage affichage

**11.4 — Coup accélérateur pleine charge (WOT rapide):**
- [ ] RPM monte rapidement sans perte signal
- [ ] Pas de reset Innovate
- [ ] Pas de freeze affichage

**11.5 — Essai routier 30min (conditions réelles):**
- [ ] Affichage RPM stable toutes conditions
- [ ] Température habitacle: noter _______ °C
- [ ] Aucun comportement anormal
- [ ] Moteur fonctionne normalement

**11.6 — Température composants (après 30min roulage):**
- Arrêt moteur, ouverture boîtier circuit (si accessible)
- Température R1: _______ °C, GO (< 70°C): ✅ / NO-GO: ❌
- Température U1: _______ °C, GO (< 60°C): ✅ / NO-GO: ❌

---

## SECTION 12 — CHECKLIST GO / NO-GO FINALE

### Critères validation fabrication

**Tests banc (Section 4 à 10):**
- [ ] Section 4: Alimentation 12V GO
- [ ] Section 5: Régulateur 5V GO
- [ ] Section 6: VTACH repos GO
- [ ] Section 7: Signal simulé GO
- [ ] Section 9: Thermique R1 GO
- [ ] Section 10: Immunité GO

**Tests véhicule (Section 11):**
- [ ] Ralenti stable GO
- [ ] Montée RPM linéaire GO
- [ ] WOT suivi GO
- [ ] Essai 30min GO
- [ ] Températures < limites GO

**Inspection post-essai:**
- [ ] Aucune trace surchauffe PCB
- [ ] Fusibles F1 et lame 1A intacts
- [ ] Câblage en bon état
- [ ] Pas d'odeur anormale
- [ ] Borniers serrés fermement

### DÉCISION FINALE

**STATUT:** 
- [ ] ✅ GO FABRICATION — Circuit conforme, prêt utilisation
- [ ] ❌ NO-GO — Anomalie détectée, voir diagnostic Annexe A

**Validé par:** _______________________  
**Date:** _____ / _____ / _____  
**Signature:** _______________________

---

## ANNEXE A — DIAGNOSTIC PANNES

### A.1 — Pas de tension +12V_PROT_PWR

**Symptôme:** 0V mesuré, Section 4 NO-GO

**Causes possibles:**
1. Fusible F1 grillé → Remplacer, vérifier court-circuit aval
2. D1 (1N5822) coupée → Tester diode isolée, remplacer si OL
3. Câblage J1 défaillant → Vérifier continuité batterie → J1

### A.2 — Pas de tension +5V

**Symptôme:** 0V ou < 4,5V, Section 5 NO-GO

**Causes possibles:**
1. U1 (R-78E) défaillant → Mesurer VIN pin1 (doit être ~13V), remplacer U1
2. Court-circuit sur +5V → Déconnecter charge, mesurer résistance +5V→GND
3. C5 en court-circuit → Déssouder C5, tester isolé

### A.3 — VTACH bloqué 0V (repos)

**Symptôme:** VTACH < 1V sans signal, Section 6 NO-GO

**Causes possibles:**
1. H11L1 conduit à tort → LED entrée polarisée (D6 inversée?)
2. R5 (6,8kΩ) coupée → Tester résistance isolée
3. Court-circuit VTACH → GND piste PCB

### A.4 — VTACH bloqué 5V (signal présent)

**Symptôme:** VTACH reste haut avec injection signal, Section 7 NO-GO

**Causes possibles:**
1. H11L1 ne commute pas → Vérifier LED H11L1 s'allume (visible IR)
2. R3 (680Ω) coupée → Pas de courant LED
3. D5 (Zener 5,1V) en court-circuit → Clamp signal entrée H11L1

### A.5 — Surchauffe R1

**Symptôme:** Température > 80°C, Section 9 NO-GO

**Causes possibles:**
1. Signal bobine > -300V → Vérifier amplitude oscilloscope
2. R1 sous-dimensionnée → Confirmer 5W MOX
3. Mauvaise dissipation thermique → Éloigner R1 du PCB (pattes longues)

### A.6 — RPM véhicule instable (double comptage)

**Symptôme:** Affichage Innovate saute ×2, Section 11 NO-GO

**Causes possibles:**
1. Bruit EMI capté → Ajouter ferrites FB1/FB2 si absentes
2. Mauvaise masse → Vérifier Point A, Point B, jonction châssis
3. Câble VTACH non blindé → Utiliser câble blindé torsadé

---

## ANNEXE B — RÉFÉRENCES COMPOSANTS

### Semiconducteurs (sources recommandées)

| Composant | Fabricant | P/N | Distributeur |
|-----------|-----------|-----|--------------|
| R-78E5.0-0.5 | RECOM | R-78E5.0-0.5 | Mouser, DigiKey |
| H11L1M | Vishay | H11L1M | Mouser, Farnell |
| 1N5822 | Vishay | 1N5822-E3/54 | Mouser, DigiKey |
| 15KPA18CA | Littelfuse | 15KPA18CA | Mouser, LCSC |
| ZY27 | Diotec | ZY27 | Farnell, TME |
| 1.5KE250CA | Littelfuse | 1.5KE250CA | Mouser |
| 1N4733A | Vishay | 1N4733A-E3/73 | Mouser |
| SA5.0CA | Littelfuse | SA5.0CA | Mouser |

### Résistances haute puissance

| Valeur | Puissance | Type | Fabricant | P/N |
|--------|-----------|------|-----------|-----|
| 47kΩ | 5W | Metal Oxide | Vishay | AC05000004709JAC00 |
| 1kΩ | 3W | Metal Oxide | Vishay | AC03000001001JAC00 |

---

## ANNEXE C — CALCULS DIMENSIONNEMENT

### C.1 — Limitation courant R1

**Pire cas bobine:** -450V, durée pulse 2ms, fréquence 6000 RPM (100Hz)

**Courant pic LED H11L1 (IF_peak):**
```
IF_peak = (|V_bobine| - V_zener_D5 - V_f_LED) / (R1 + R2 + R3)
IF_peak = (450V - 5,1V - 1,2V) / (47000Ω + 1000Ω + 680Ω)
IF_peak = 443,7V / 48680Ω = 9,1 mA
```

**Conforme:** IF_max H11L1 = 60mA → Marge ×6,6

### C.2 — Puissance dissipée R1

**Puissance moyenne:**
```
P_moy = (V_R1²/R1) × DutyCycle
V_R1 ≈ 443,7V × (47kΩ / 48,68kΩ) = 428V
P_moy = (428² / 47000) × 0,02 (2% duty 6000 RPM) = 0,078W
```

**Température élévation (θ = 50°C/W boîtier libre):**
```
ΔT = P × θ = 0,078W × 50°C/W = 3,9°C
T_finale = T_amb + ΔT = 25°C + 3,9°C = 28,9°C
```

**Conforme:** << 250°C (T_max R1)

### C.3 — Capacité filtrage π

**Impédance condensateurs à 100kHz (bruit switching):**
```
Z_C3 = 1 / (2π × 100kHz × 100nF) = 15,9Ω
```

**Atténuation bruit (avec L1 = 22µH):**
```
f_coupure = 1 / (2π × √(L1 × C3))
f_coupure = 1 / (2π × √(22µH × 100nF)) = 10,7 kHz
```

**Atténuation 100kHz:** -40dB/decade → ~-38dB

---

## ANNEXE D — INTERPRÉTATION OSCILLOSCOPE

### Forme d'onde typique VTACH

```
Signal bobine (J2 pin1):
     0V ────────┐              ┐
                │              │
                │ 2ms          │
   -300V        └──────────────┘
                <─── 10ms ───>  (100Hz, 6000 RPM)

Signal VTACH (J3 pin1):
                ┌──────────────┐
     5V         │              │
                │              │ Logique inversée
     0V ────────┘              └────
                <─ 2ms ─>
```

### Anomalies détectables

**Oscillations haute fréquence sur VTACH:**
- Cause: C7 (15nF) absent ou trop faible
- Solution: Vérifier C7 soudé

**Pente lente montée/descente (>50µs):**
- Cause: R4 trop élevé ou C7 trop gros
- Solution: Vérifier R4 = 470Ω, C7 = 15nF

**Rebonds (multiples pulses):**
- Cause: Bruit EMI couplé, masses mal étoilées
- Solution: Ajouter ferrites, vérifier Point A/B

---

## ANNEXE E — SPÉCIFICATIONS INNOVATE MTX-D

### Entrée RPM (d'après manuel Innovate)

| Paramètre | Valeur |
|-----------|--------|
| Type entrée | Logique 0-5V CMOS |
| Impédance | > 100kΩ |
| Seuil bas (0) | < 1,5V |
| Seuil haut (1) | > 3,5V |
| Fréquence max | 500Hz (30 000 RPM) |
| Mode | Comptage fronts descendants |

**Calibration Innovate:**
- Menu RPM → PPR (Pulses Per Revolution)
- Moteur 4 cylindres: PPR = 2 (2 pulses/tour vilebrequin)
- Moteur 6 cylindres: PPR = 3
- Moteur 8 cylindres: PPR = 4

---

## ANNEXE F — HISTORIQUE VERSIONS PROTOCOLE

### V2.7.3 (01 Décembre 2025)
- Ajout Section 2bis: Fusible lame 1A véhicule
- Clarification température R1 limites Section 9
- Correction typo Annexe B: "15KPA18CA"

### V2.7.2 (28 Novembre 2025)
- Ajout test immunité ESD Section 10.2
- Précision calcul puissance R1 Annexe C.2

### V2.7.1 (25 Novembre 2025)
- Ajout tableau relevés température Section 9
- Correction référence D3 "ZY27 (Diotec)"

### V2.7.0 (20 Novembre 2025)
- Refonte complète protocole (fusion V2.6.x)
- Ajout Section 11 validation véhicule terrain
- Ajout Annexe D interprétation oscilloscope

---

**FIN PROTOCOLE TEST RPM V2.7.3**
