# CIRCUIT RPM ADAPTER V5.4.12.1

**Version:** 5.4.12.1  
**Date:** 01 Décembre 2025  
**Auteur:** Mehdi  
**Application:** Ford Mustang EcoBoost 2016 Stage 4+ → Innovate MTX-D  
**Conformité:** ✅ PREMORTEM V3.4 VALIDÉ  
**Validation externe:** ✅ Diode roue libre L1 ajoutée  
**Température opération:** -40°C à +85°C (habitacle, Grade 3 AEC-Q100)

---

## CHANGELOG V5.4.12.1

**Corrections documentation uniquement:**
- Changelog V5.4.11: "Type T (time-delay)" → "Fast-blow" (cohérence avec BOM)
- D3: Clarification "ZY27 (Diotec) Zener 27V 2W" dans BOM

**Aucun changement technique.**

**Confiance:** 9,6/10  
**Statut:** ✅ PRÊT FABRICATION

---

## ARCHITECTURE GLOBALE

[BATTERIE 12V] → Fusible → Protection → Filtrage π → R-78E → +5V
                                                        ↓
[BOBINE IGNITION -300V] → Ferrites → Limitation → TVS/Zener → H11L1 → VTACH → Innovate

**Zones physiques:**
- **Habitacle:** Module complet (alim + logique)
- **Moteur:** Fil unique vers bobine ignition

---

## BLOC A — PROTECTION ENTRÉE 12V

### Topologie

**Chemin principal:**
+BATT → F1 (500mA Fast-blow) → +12V_FUSE
+12V_FUSE → D1 anode (1N5822)
D1 cathode → +12V_PROT

**Protection surtension:**
+12V_PROT → D2 TVS (15KPA18CA bidirectionnelle) → GND

**Filtrage π complet avec roue libre:**
+12V_PROT → C_pré (100nF X7R) → GND
+12V_PROT → PTC (RUEF050 500mA) → +12V_PTC
+12V_PTC → L1 (10-22µH blindée) → +12V_FILT

**Diode roue libre L1:**
+12V_PROT → D_flyback anode (1N5822)
D_flyback cathode → +12V_FILT

+12V_FILT → +12V_PROT_PWR
+12V_PROT_PWR → C1 (22µF 50V électrolytique) → GND
+12V_PROT_PWR → C2 (22µF 50V électrolytique) → GND
+12V_PROT_PWR → C3 (100nF X7R) → GND

**Snubber optionnel (empreintes prévues):**
+12V_PROT → R_snub (1Ω 0,25W) → NODE_RC
NODE_RC → C_snub (100nF) → +12V_FILT

**Protection Zener régulateur:**
+12V_PROT_PWR → D3 cathode (Zener ZY27 27V 2W)
D3 anode → GND

### Composants

**Protection:**
- F1: Fusible 500mA Verre 5×20mm Fast-blow
- D1: 1N5822 Schottky 40V 3A, DO-201AD, bague cathode
- D_flyback: 1N5822 Schottky 40V 3A, DO-201AD (roue libre L1)
- D2: 15KPA18CA TVS 18V 15000W bidirectionnelle, DO-201AE
- D3: ZY27 (Diotec) Zener 27V 2W, DO-41, bague cathode

**Filtrage:**
- PTC: RUEF050 500mA hold, 30V, Bourns
- L1: 10-22µH blindée
- C_pré: 100nF X7R 50V céramique
- C1, C2: 22µF 50V électrolytique low-ESR
- C3: 100nF X7R 50V céramique

---

## BLOC B — RÉGULATEUR 5V

### Topologie

+12V_PROT_PWR → U1 pin1 (VIN)
U1 pin2 (GND) → GND
U1 pin3 (VOUT) → +5V

+5V → C4 (100nF X7R) → GND
+5V → C5 (22µF 16V électrolytique) → GND

### Composants

- U1: R-78E5.0-0.5 régulateur 5V 500mA, SIP-3
- C4: 100nF X7R 16V céramique
- C5: 22µF 16V électrolytique low-ESR

---

## BLOC C — SIGNAL BOBINE PRIMAIRE

### Topologie

[BOBINE_BRUT] → FB1 (2 tours) → FB2 (2 tours) → [BOBINE_BRUT_FILT]
[BOBINE_BRUT_FILT] → R1 (47kΩ 5W MOX) → [POINT_CLAMP]
[POINT_CLAMP] → R2 (1kΩ 3W MOX) → [BOBINE_PROTÉGÉ]
[BOBINE_BRUT_FILT] → TVS D4 (1.5KE250CA) → CHÂSSIS
[BOBINE_PROTÉGÉ] → D5 cathode (Zener 5,1V 1W) → GND
[BOBINE_PROTÉGÉ] → R3 (680Ω) → H11L1 pin1
H11L1 pin1 → D6 cathode (1N4148) → H11L1 pin2 → GND

### Composants

- FB1, FB2: Ferrite anneau 2 tours
- R1: 47kΩ 5W Metal Oxide (MOX)
- R2: 1kΩ 3W Metal Oxide (MOX)
- R3: 680Ω 0,25W Metal Film
- D4: 1.5KE250CA TVS 250V 1,5kW bidir
- D5: 1N4733A Zener 5,1V 1W
- D6: 1N4148 protection LED inverse
- U2: H11L1M optocoupleur Schmitt

---

## BLOC D — H11L1 LOGIQUE (SORTIE)

### Topologie

+5V → H11L1 pin6 → C6 (100nF) → pin5 → GND
H11L1 pin4 → R4 (470Ω) → [VTACH]
[VTACH] → R5 (6,8kΩ) → +5V
[VTACH] → C7 (15nF) → GND
[VTACH] → D7 (SA5.0CA) → GND

### Composants

- C6: 100nF X7R 16V céramique
- C7: 15nF X7R 50V céramique
- R4: 470Ω 0,25W Metal Film
- R5: 6,8kΩ 0,25W Metal Film
- D7: SA5.0CA TVS 5V bidir ESD

---

## BLOC J — CONNECTEURS

### Topologie

**J1 (Bornier alimentation):**
Pin1 (+12V) → F1 → Circuit
Pin2 (GND) → Point B (masse puissance)

**J2 (Bornier bobine):**
Pin1 (SIGNAL) → FB1 → Circuit
Pin2 (GND) → CHÂSSIS direct

**J3 (Sortie Innovate):**
Pin1 (VTACH) → Innovate RPM input
Pin2 (GND) → Point A (masse logique)

### Masses étoile

**Point A (masse logique):** R-78E, H11L1, condensateurs +5V, D5
**Point B (masse puissance):** TVS D2/D4, condensateurs filtrage, J1 GND

**Jonction unique:** Point A → Point B → CHÂSSIS

---

## BOM COMPLÈTE

### Semiconducteurs

| Réf | Composant | Valeur | Package |
|-----|-----------|--------|---------|
| U1 | R-78E5.0-0.5 | 5V 500mA | SIP-3 |
| U2 | H11L1M | Opto Schmitt | DIP-6 |
| D1 | 1N5822 | Schottky 40V | DO-201AD |
| D_flyback | 1N5822 | Schottky 40V | DO-201AD |
| D2 | 15KPA18CA | TVS 18V | DO-201AE |
| D3 | ZY27 (Diotec) | Zener 27V | DO-41 |
| D4 | 1.5KE250CA | TVS 250V | DO-201AE |
| D5 | 1N4733A | Zener 5,1V | DO-41 |
| D6 | 1N4148 | Diode signal | DO-35 |
| D7 | SA5.0CA | TVS 5V | DO-214AC |

### Résistances

| Réf | Valeur | W | Type |
|-----|--------|---|------|
| R1 | 47kΩ | 5W | Metal Oxide |
| R2 | 1kΩ | 3W | Metal Oxide |
| R3 | 680Ω | 0,25W | Metal Film |
| R4 | 470Ω | 0,25W | Metal Film |
| R5 | 6,8kΩ | 0,25W | Metal Film |

### Condensateurs

| Réf | Valeur | V | Type |
|-----|--------|---|------|
| C_pré | 100nF | 50V | X7R |
| C1, C2 | 22µF | 50V | Électrolytique |
| C3 | 100nF | 50V | X7R |
| C4 | 100nF | 16V | X7R |
| C5 | 22µF | 16V | Électrolytique |
| C6 | 100nF | 16V | X7R |
| C7 | 15nF | 50V | X7R |

### Protections

| Réf | Type | Specs |
|-----|------|-------|
| F1 | Verre 5×20mm | 500mA Fast-blow |
| PTC | RUEF050 | 500mA, 30V |
| L1 | Blindée | 10-22µH |
| FB1, FB2 | Ferrite | 2 tours |

---

**FIN CIRCUIT RPM V5.4.12.1**
