# 📋 Bill of Materials (BOM) — RPM Adapter Circuit

**Version:** V5.4.12. 1  
**Date:** Décembre 2025  
**Statut:** ✅ VALIDÉ — PRÊT FABRICATION

---

## Semiconducteurs

| Réf | Composant | Valeur | Fonction | Qty |
|-----|-----------|--------|----------|-----|
| U1 | R-78E5. 0-0.5 | 5V 500mA | Régulateur DC-DC | 1 |
| U2 | H11L1M | Opto Schmitt | Isolation galvanique | 1 |
| D1 | 1N5822 | Schottky 40V 3A | Anti-inversion | 1 |
| D_flyback | 1N5822 | Schottky 40V 3A | Roue libre L1 | 1 |
| D2 | 15KPA18CA | TVS 18V 15kW | Protection load-dump | 1 |
| D3 | ZY27 (Diotec) | Zener 27V 2W | Protection régulateur | 1 |
| D4 | 1.5KE250CA | TVS 250V 1,5kW | Protection bobine | 1 |
| D5 | 1N4733A | Zener 5,1V 1W | Clamp signal | 1 |
| D6 | 1N4148 | Signal 100V | Protection LED inverse | 1 |
| D7 | SA5. 0CA | TVS 5V ESD | Protection sortie | 1 |

---

## Résistances

| Réf | Valeur | Puissance | Type | Note | Qty |
|-----|--------|-----------|------|------|-----|
| R1 | 47kΩ | 5W | Metal Oxide (MOX) | **V_rating ≥ 750V** ⚠️ | 1 |
| R2 | 1kΩ | 3W | Metal Oxide (MOX) | — | 1 |
| R3 | 680Ω | 0,25W | Metal Film | Courant LED | 1 |
| R4 | 470Ω | 0,25W | Metal Film | Série sortie | 1 |
| R5 | 6,8kΩ | 0,25W | Metal Film | Pull-up | 1 |

---

## Condensateurs

| Réf | Valeur | Tension | Type | Qty |
|-----|--------|---------|------|-----|
| C1, C2 | 22µF | 50V | Électrolytique low-ESR | 2 |
| C3, C4, C6, C_pré | 100nF | 50V | X7R céramique | 4 |
| C5 | 22µF | 16V | Électrolytique low-ESR | 1 |
| C7 | **15nF** | 50V | X7R céramique | 1 |

---

## Protections & Passifs

| Réf | Type | Specs | Qty |
|-----|------|-------|-----|
| F1 | Fusible verre 5×20mm | 500mA **Fast-blow** | 1 |
| PTC | RUEF050 (Bourns) | 500mA hold, 30V | 1 |
| L1 | Inductance blindée | 10-22µH | 1 |
| FB1, FB2 | Ferrite anneau | 2 tours | 2 |

---

## Connecteurs

| Réf | Type | Fonction | Qty |
|-----|------|----------|-----|
| J1 | Bornier 2 positions | Alimentation 12V | 1 |
| J2 | Bornier 2 positions | Entrée signal bobine | 1 |
| J3 | Bornier 2 positions | Sortie VTACH vers Innovate | 1 |

---

## ⚠️ Notes importantes

- **R1** : V_rating ≥ 750V obligatoire, Metal Oxide (MOX)
- **F1** : Fast-blow obligatoire (pas slow-blow)
- **TOTAL composants** : 31

---

**FIN BOM**
