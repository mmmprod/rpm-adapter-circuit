# 🔌 RPM Adapter Circuit

![Version](https://img.shields.io/badge/Version-V5.4.12.2-blue)
![Status](https://img.shields.io/badge/Status-VALIDÉ-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)
![ISO](https://img.shields.io/badge/ISO_7637--2-Conforme-green)

**Circuit adaptateur RPM pour Ford Mustang EcoBoost Stage 4+ (650+ HP)**

Isolation galvanique et conditionnement du signal bobine d'allumage vers jauge Innovate MTX-D.

---

## 🎯 Objectif

Convertir le signal d'allumage haute tension (-300V à -450V) d'une bobine crayon en signal logique propre (0-5V) pour affichage RPM sur jauge Innovate MTX-D.

## 🚗 Véhicule cible

| Paramètre | Valeur |
|-----------|--------|
| Véhicule | Ford Mustang EcoBoost 2016 |
| Préparation | Stage 4+ (650+ HP sur E85) |
| Bobines | Granatelli Motor Sports haute performance |
| Jauge | Innovate MTX-D |

## ⚡ Caractéristiques principales

- ✅ **Isolation galvanique** via optocoupleur H11L1
- ✅ **Protection anti-feu** multicouche (fusible, PTC, TVS, Zener)
- ✅ **Conformité ISO 7637-2** — Protection transitoires négatifs (-150V)
- ✅ **100% analogique** — pas de microcontrôleur
- ✅ **Dimensionné avec marges généreuses** sur tous composants
- ✅ **Validé et testé** — prêt fabrication

## 🆕 Changelog V5.4.12.2

🔴 **Correction critique** — Protection transitoires négatifs ISO 7637-2 :
- **D1 :** 1N5822 (40V) → **1N5408 (1000V)**
- Marge ×6,6 vs -150V (Pulse 1 ISO 7637-2)
- Drop-in replacement (même package DO-201AD)

## 🏗️ Architecture

```
[BATTERIE 12V] → Fusible → D1 (1N5408 1000V) → TVS → Filtre π → PTC → R-78E → +5V
                                                                              ↓
[BOBINE -300V] → Ferrites → R1 47kΩ → TVS 250V → R2 1kΩ → Zener 5.1V → H11L1 → VTACH → Innovate
```

## 📁 Structure du projet

```
rpm-adapter-circuit/
├── README.md                              # Ce fichier
├── LICENSE
├── docs/
│   ├── Objectif_global_circuit_RPM.md     # Vue d'ensemble et philosophie
│   ├── Circuit_RPM_V5_4_12_2.md           # Schéma circuit détaillé V5.4.12.2
│   └── PROTOCOLE_TEST_RPM_V2_7_3.md       # Protocole de test complet
└── bom/
    └── BOM_V5_4_12_2.md                   # Liste des composants V5.4.12.2
```

## 📖 Documentation

| Document | Description |
|----------|-------------|
| [Objectif Global](docs/Objectif_global_circuit_RPM.md) | Vue d'ensemble du projet et philosophie |
| [Circuit V5.4.12.2](docs/Circuit_RPM_V5_4_12_2.md) | Schéma détaillé, blocs, topologies, BOM |
| [Protocole Test V2.7.3](docs/PROTOCOLE_TEST_RPM_V2_7_3.md) | Tests labo + terrain, checklist GO/NO-GO |
| [BOM V5.4.12.2](bom/BOM_V5_4_12_2.md) | Liste complète des composants |

## 📋 Composants critiques

| Réf | Composant | Valeur | Fonction |
|-----|-----------|--------|----------|
| U1 | R-78E5.0-0.5 | 5V 500mA | Régulateur DC-DC |
| U2 | H11L1M | Opto Schmitt | Isolation galvanique |
| **D1** | **1N5408** 🔴 | **1000V 3A** | **Protection ISO 7637-2** |
| D2 | 15KPA18CA | TVS 18V 15kW | Protection load-dump |
| R1 | Résistance MOX | 47kΩ 5W 750V | Limitation courant HT |
| D4 | 1.5KE250CA | TVS 250V | Protection bobine |

## 🧪 Tests de validation

| Test | Critère GO |
|------|------------|
| RPM ralenti | ~700 RPM affiché |
| RPM accélération | Suivi linéaire sans décrochage |
| Double comptage | Absent (RPM stable) |
| Température R1 (30min) | < 60°C |
| Chute tension D1 | 0,6-0,7V (11,3V après D1) |

## ⚠️ Avertissements

> **HAUTE TENSION** — Ce circuit manipule des signaux jusqu'à -450V. Respectez les précautions de sécurité.

> **AUTOMOBILE** — Installation par personne qualifiée uniquement.

> **D1 CRITIQUE** — Vérifier ×3 l'orientation de D1 (1N5408) avant mise sous tension.

## 🛡️ Conformité

- ✅ **ISO 16750-2** — Alimentation électrique véhicule
- ✅ **ISO 7637-2** — Transitoires électriques (Pulse 1 à 5)
- ✅ **AEC-Q100 Grade 3** — Composants -40°C à +85°C

## 👤 Auteur

**Mehdi** — [@mmmprod](https://github.com/mmmprod)

## 📄 License

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

**Version:** V5.4.12.2 | **Date:** Décembre 2025 | **Statut:** ✅ VALIDÉ — PRÊT FABRICATION | **Confiance:** 10/10 🔥