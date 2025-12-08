# OBJECTIF GLOBAL — CIRCUIT RPM ADAPTER

**Version document:** 2.0  
**Date:** 03 Décembre 2025  
**Circuit référence:** V5.4.12.1  
**Statut:** ✅ VALIDÉ — PRÊT FABRICATION  
**Auteur:** Mehdi

---

## INFORMATIONS CONTEXTUELLES POUR IA

### Véhicule cible

| Paramètre | Valeur |
|-----------|--------|
| Véhicule | Ford Mustang EcoBoost 2016 |
| Préparation | Stage 4+ (650+ HP sur E85) |
| Bobines | Granatelli Motor Sports haute performance |
| Pics tension bobine | -300V à -450V (mesuré oscilloscope) |
| Jauge destination | Innovate MTX-D |

### Conditions d'utilisation réelles

| Paramètre | Valeur |
|-----------|--------|
| Pays | France |
| Température min | -10°C (pas -40°C) |
| Température max | +70°C |
| Emplacement module | Habitacle, console centrale |
| Environnement | Protégé, ventilé naturellement |
| Longueur câble VTACH | ~1m vers Innovate |

### Philosophie de conception

| Principe | Application |
|----------|-------------|
| Anti-feu | Priorité absolue, protections redondantes |
| Isolation galvanique | Optocoupleur H11L1 sépare moteur/logique |
| Dimensionnement | Marges généreuses sur tous composants |
| Simplicité | Pas de microcontrôleur, 100% analogique |

---

## OBJECTIF FONCTIONNEL

Le circuit prend le signal d'allumage d'une bobine crayon moteur (côté commande négatif), le nettoie et le rend propre, stable et sûr pour l'entrée compte-tours de la jauge Innovate MTX-D.

**Entrée:** Signal bobine ignition -300V à -450V, fronts raides, bruit EMI intense  
**Sortie:** Signal logique VTACH 0-5V, impulsions propres, isolé galvaniquement  
**Protection:** Aucun risque de chauffe, incendie ou destruction de l'Innovate

---

## ARCHITECTURE GLOBALE

```
[BATTERIE 12V] → Fusible → Schottky → TVS → Filtre π → PTC → R-78E → +5V
                                                                    ↓
[BOBINE -300V] → Ferrites → R1 47kΩ → TVS 250V → R2 1kΩ → Zener 5.1V → H11L1 → VTACH → Innovate
```

**Zones physiques:**
- **Habitacle:** Module complet (alimentation + logique)
- **Moteur:** Fil unique vers bobine ignition (blindé)

---

## CHEMIN DU SIGNAL RPM — CÔTÉ MOTEUR

### Étape 1 — Récupération signal

On récupère le signal sur la borne négative d'une bobine d'allumage individuelle (coil-on-plug). Ce point est très agressif électriquement: crêtes haute tension rapides (-300V à -450V), fronts très raides, bruit EMI typique injection directe/turbo.

### Étape 2 — Ferrites d'entrée

Le fil passe par deux tours dans des ferrites (FB1 et FB2) pour atténuer les pics très rapides (HF) avant l'électronique.

### Étape 3 — Limitation courant R1

**Composant:** R1 = 47kΩ 5W Metal Oxide (MOX), V_rating ≥ 750V

**Rôle:** Limiter sévèrement le courant en cas de surtension. L'énergie se dissipe dans cette résistance plutôt que dans le reste du circuit.

**Dimensionnement validé:**
- V_rating: 750V > V_pic 450V → Marge 1,67×
- SOTV (pulse): 1215V > V_pic 450V → Marge 2,7×
- Puissance: 5W >> P_moyenne 0,04W → Marge 125×

### Étape 4 — TVS haute tension D4

**Composant:** D4 = 1.5KE250CA TVS bidirectionnelle 250V 1,5kW

**Rôle:** Plafonner les surtensions positives et négatives. Empêcher qu'un pic allumage >250V parte tuer le reste du montage.

**Placement critique:** <5cm du connecteur J2, retour CHÂSSIS direct (pas GND logique).

### Étape 5 — Limitation secondaire R2 + Zener D5

**Composants:**
- R2 = 1kΩ 3W MOX
- D5 = 1N4733A Zener 5,1V 1W

**Rôle:** Limiter le signal dans une fenêtre sûre (0-5V), sans pics négatifs violents.

### Étape 6 — Optocoupleur H11L1

**Composants:**
- R3 = 680Ω (courant LED)
- U2 = H11L1M optocoupleur Schmitt trigger
- D6 = 1N4148 protection LED inverse (antiparallèle)

**Calcul courant LED:**
```
I_LED = (5,1V - 1,2V) / 680Ω = 5,7mA
IF(ON) requis H11L1 = 1,6mA
Marge = 5,7mA / 1,6mA = 3,6× ✅
```

**Rôle clé:** À ce stade, toute l'énergie "dégueulasse" du moteur est brûlée, écrêtée ou absorbée. L'isolation galvanique protège complètement l'Innovate.

---

## MISE EN FORME LOGIQUE — CÔTÉ PROPRE

### Sortie H11L1

Côté secondaire (sortie transistor), l'opto est alimenté en +5V régulés propres. Quand la bobine fait son événement d'allumage, l'opto commute et sort une impulsion logique nette.

### Réseau de conditionnement VTACH

**Composants:**
- R4 = 470Ω série (limite courant crête, calme les fronts)
- R5 = 6,8kΩ pull-up vers +5V (impulsions 0-5V)
- C7 = **15nF** X7R (filtre HF, évite double comptage)
- D7 = SA5.0CA TVS ESD bidirectionnelle

### Calcul temps de montée

```
t_rise = R5 × C7 = 6,8kΩ × 15nF = 102µs
f_max = 1 / (2 × t_rise) = 4,9kHz
RPM_max = 4900Hz × 60s / 2 = 147 000 RPM >> 6500 RPM ✅
```

### Résultat

VTACH est un signal carré propre:
- Amplitude: 0V à 5V
- Niveau bas: ~0,6V (sûr sous seuil LOW)
- Limité en bruit HF
- Protégé contre l'ESD
- Exploitable directement par l'Innovate

---

## ALIMENTATION ET PROTECTIONS FEU

### Chaîne d'alimentation

Le circuit logique est alimenté à partir du 12V véhicule, mais **jamais directement**. Chaîne de protection complète:

**1. Fusible F1 (500mA Fast-blow)**
- Placé très près de la prise 12V
- Limite l'énergie disponible en cas de court-circuit

**2. Diode Schottky D1 (1N5822)**
- Clapet anti-retour
- Empêche courant inverse si polarité inversée

**3. TVS D2 (15KPA18CA)**
- Absorbe surtensions transitoires (load-dump, pics alternateur)
- Capacité: 15kW pour 10/1000µs
- V_clamp ≈ 29V (protège condensateurs 50V)

**4. Filtre π**
- C_pré (100nF) → L1 (10-22µH blindée) → C1/C2 (22µF 50V)
- Filtre bruit alternateur, injecteurs, turbo

**5. Diode roue libre D_flyback (1N5822)**
- Parallèle sur L1
- Protège PTC contre spikes inductifs lors trip

**6. PTC RUEF050 (500mA)**
- Si court-circuit interne, PTC grimpe en résistance
- Limite l'énergie dissipée → évite incendie

**7. Zener D3 (ZY27 Diotec 27V 2W)**
- Protection redondante entrée régulateur
- Limite à 27V si TVS défaillante

**8. Régulateur R-78E5.0-0.5**
- Entrée: 7-28V (12V filtré)
- Sortie: +5V stable 500mA
- Efficacité: 90% typique

### Philosophie anti-feu

| Protection | Fonction |
|------------|----------|
| Fusible 500mA | Limite énergie max disponible |
| Diode série | Empêche erreurs polarité / retour courant |
| PTC 500mA | Coupe-circuit interne réarmable |
| R1 5W MOX | Dissipe pics sans carboniser |
| TVS multiples | Clampent surtensions, pas de conduction permanente |
| Isolation galvanique | Sépare monde moteur (violent) du compteur (fragile) |

---

## GESTION DES MASSES

### Point A — Masse logique

- R-78E pin2 (GND)
- H11L1 pin2 (GND LED) et pin5 (GND logique)
- Condensateurs +5V
- D5 anode (Zener 5,1V)
- J3 GND (sortie Innovate)

### Point B — Masse puissance

- TVS D2, D4 retour
- Condensateurs filtrage π
- J1 GND (alimentation 12V)

### Jonction unique

Point A → Point B → CHÂSSIS véhicule

**Critère validation:** ΔV(GND) module-châssis ≤ 0,10V DC

### Câble VTACH

- Câble blindé ~1m
- Blindage raccordé **uniquement côté Innovate**
- Évite boucles de masse
- Réduit bruit RF et re-déclenchements parasites

---

## BOM COMPOSANTS CRITIQUES

### Semiconducteurs

| Réf | Composant | Valeur | Fonction |
|-----|-----------|--------|----------|
| U1 | R-78E5.0-0.5 | 5V 500mA | Régulateur DC-DC |
| U2 | H11L1M | Opto Schmitt | Isolation galvanique |
| D1 | 1N5822 | Schottky 40V 3A | Anti-inversion |
| D_flyback | 1N5822 | Schottky 40V 3A | Roue libre L1 |
| D2 | 15KPA18CA | TVS 18V 15kW | Protection load-dump |
| D3 | ZY27 (Diotec) | Zener 27V 2W | Protection régulateur |
| D4 | 1.5KE250CA | TVS 250V 1,5kW | Protection bobine |
| D5 | 1N4733A | Zener 5,1V 1W | Clamp signal |
| D6 | 1N4148 | Signal 100V | Protection LED inverse |
| D7 | SA5.0CA | TVS 5V ESD | Protection sortie |

### Résistances

| Réf | Valeur | Puissance | Type | Note |
|-----|--------|-----------|------|------|
| R1 | 47kΩ | 5W | Metal Oxide | **V_rating ≥ 750V** |
| R2 | 1kΩ | 3W | Metal Oxide | — |
| R3 | 680Ω | 0,25W | Metal Film | Courant LED |
| R4 | 470Ω | 0,25W | Metal Film | Série sortie |
| R5 | 6,8kΩ | 0,25W | Metal Film | Pull-up |

### Condensateurs

| Réf | Valeur | Tension | Type |
|-----|--------|---------|------|
| C1, C2 | 22µF | 50V | Électrolytique low-ESR |
| C3, C4, C6, C_pré | 100nF | 50V | X7R céramique |
| C5 | 22µF | 16V | Électrolytique low-ESR |
| C7 | **15nF** | 50V | X7R céramique |

### Protections

| Réf | Type | Specs |
|-----|------|-------|
| F1 | Verre 5×20mm | 500mA **Fast-blow** |
| PTC | RUEF050 | 500mA hold, 30V, Bourns |
| L1 | Inductance blindée | 10-22µH |
| FB1, FB2 | Ferrite | 2 tours anneau |

---

## DÉCISIONS DE CONCEPTION VALIDÉES

### Pourquoi R1 = 47kΩ 5W MOX 750V (pas 2×22kΩ)

| Critère | Valeur |
|---------|--------|
| V_rating | 750V |
| V_pic bobine | 400-450V max |
| Marge tension | 1,67× ✅ |
| SOTV (pulse) | 1215V → Marge 2,7× |
| Puissance | 5W >> 0,04W utilisé |

**Conclusion:** Une seule résistance 750V suffit largement. Pas besoin de 2×22kΩ série.

### Pourquoi C1/C2 = 50V (pas 63V)

| Paramètre | Valeur |
|-----------|--------|
| V_load-dump brut | 35V |
| V_clamp TVS D2 | ~29V |
| V sur condensateurs | 29V max (après TVS) |
| Marge 50V | 50V / 29V = 1,72× ✅ |

**Conclusion:** La TVS D2 clamp à 29V. Les condensateurs 50V ont une marge suffisante.

### Pourquoi R3 = 680Ω (pas 470Ω)

| Paramètre | Valeur |
|-----------|--------|
| I_LED avec 680Ω | 5,7mA |
| IF(ON) requis H11L1 | 1,6mA |
| Marge | 3,6× ✅ |
| Température min réelle | -10°C (France) |
| CTR à -10°C | >80% nominal |

**Conclusion:** Pour -10°C (France), 680Ω donne une marge suffisante. 470Ω serait utile pour -40°C (non applicable).

### Pourquoi D3 Zener 27V conservée

| Argument | Explication |
|----------|-------------|
| Redondance | Si TVS D2 défaillante, D3 limite à 27V |
| Philosophie anti-feu | Protection multicouche |
| Coût | ~0,10€ |
| Impact négatif | Aucun |

**Conclusion:** Conservée pour sécurité redondante.

### Pourquoi C7 = 15nF (pas 33nF)

| Paramètre | 15nF | 33nF |
|-----------|------|------|
| t_rise | 102µs | 224µs |
| f_max | 4,9kHz | 2,2kHz |
| RPM_max théorique | 147k | 66k |

**Conclusion:** 15nF optimise le temps de montée tout en filtrant correctement. Les deux valeurs fonctionnent, mais 15nF est préféré.

---

## TESTS DE VALIDATION OBLIGATOIRES

### Test statique (circuit OFF)

| Test | Attendu |
|------|---------|
| Continuité GND | Toutes masses reliées |
| Isolation VCC/GND | Pas de court-circuit |
| Diodes mode diode | 0,3-0,7V sens passant |
| D_flyback orientation | Rouge +12V_PROT, Noir +12V_FILT → 0,4-0,6V |

### Test alimentation

| Mesure | Attendu |
|--------|---------|
| +5V régulé | 4,90 - 5,10V |
| Ripple +5V | <50mV |
| I_repos (sans signal) | <10mA |

### Test fonctionnel terrain

| Test | Critère GO |
|------|------------|
| RPM ralenti | ~700 RPM affiché |
| RPM accélération | Suivi linéaire sans décrochage |
| Pas de double comptage | RPM stable, pas de x2 |
| Température R1 après 30min | <60°C au toucher |

---

## POINTS DE VÉRIFICATION CÂBLAGE

| # | Vérification | Importance |
|---|--------------|------------|
| 1 | D4 retourne à J2 pin2 (châssis moteur direct) | Critique |
| 2 | Blindage VTACH connecté uniquement côté Innovate | Critique |
| 3 | R1 espacée du PCB (~3mm), flux d'air autour | Important |
| 4 | TVS D2 à <5cm du bornier J1 | Important |
| 5 | Masses étoile respectées (Point A → Point B → Châssis) | Important |

---

## CE QUE SORT LE CIRCUIT

L'Innovate MTX-D reçoit sur son entrée RPM:

| Paramètre | Valeur |
|-----------|--------|
| Type signal | Impulsions carrées |
| Niveau haut | ~5V |
| Niveau bas | ~0,6V |
| Synchronisation | 1 pulse par événement allumage |
| Isolation | Galvanique (optocoupleur) |
| Protection | TVS ESD en sortie |

**Configuration Innovate:** Ajuster "pulses par tour" selon affichage réel vs réel (×2 ou ÷2 si besoin).

---

## RÉSUMÉ POUR IA

### Ce circuit est:
- ✅ Validé et testé fonctionnel
- ✅ Dimensionné avec marges généreuses
- ✅ Protégé contre incendie (multicouche)
- ✅ Isolé galvaniquement
- ✅ Prêt à fabriquer

### Ne PAS recommander:
- ❌ R1 en 2×22kΩ (R1 750V suffit)
- ❌ C1/C2 en 63V (50V OK avec TVS)
- ❌ R3 en 470Ω (680Ω OK pour France)
- ❌ Supprimer D3 (sécurité redondante)
- ❌ Séparer les masses en 3 domaines (architecture actuelle fonctionne)

### Seules améliorations optionnelles:
- 🟡 Boîtier ABS ignifugé (UL94-V0) ou alu
- 🟡 R damping 22Ω si problème de résonance câble (non observé)

---

## HISTORIQUE VALIDATIONS

| Date | Version | Validation |
|------|---------|------------|
| 01 Déc 2025 | V5.4.12.1 | ✅ Prêt fabrication |
| 03 Déc 2025 | 3 audits externes | ✅ Tous rejetés (erreurs d'analyse) |

**Confiance finale:** 99/100  
**Statut:** ✅ CIRCUIT VALIDÉ

---

**FIN DOCUMENT OBJECTIF GLOBAL**
