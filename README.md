# 🔌 RPM Adapter Circuit

![Version](https://img.shields.io/badge/Version-V5.4.12.1-blue)
![Status](https://img.shields.io/badge/Status-VALIDÉ-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)

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
- ✅ **100% analogique** — pas de microcontrôleur
- ✅ **Dimensionné avec marges généreuses** sur tous composants
- ✅ **Validé et testé** — prêt fabrication

## 🏗️ Architecture

```
[BATTERIE 12V] → Fusible → Schottky → TVS → Filtre π → PTC → R-78E → +5V
                                                                    ↓
[BOBINE -300V] → Ferrites → R1 47kΩ → TVS 250V → R2 1kΩ → Zener 5.1V → H11L1 → VTACH → Innovate
```

## 📁 Structure du projet

```
rpm-adapter-circuit/
├── README.md                 # Ce fichier
├── LICENSE
├── docs/
│   └── Objectif_global_circuit_RPM.md   # Documentation technique complète
├── schematics/               # Schémas électroniques (KiCad/Eagle)
├── pcb/                      # Fichiers de conception PCB
└── bom/
    └── BOM.md                # Liste des composants
```

## 📋 Bill of Materials (BOM)

Voir [bom/BOM.md](bom/BOM.md) pour la liste complète des composants.

### Composants critiques

| Réf | Composant | Valeur | Fonction |
|-----|-----------|--------|----------|
| U1 | R-78E5.0-0.5 | 5V 500mA | Régulateur DC-DC |
| U2 | H11L1M | Opto Schmitt | Isolation galvanique |
| R1 | Résistance MOX | 47kΩ 5W 750V | Limitation courant HT |
| D4 | 1.5KE250CA | TVS 250V | Protection bobine |

## 🧪 Tests de validation

| Test | Critère GO |
|------|------------|
| RPM ralenti | ~700 RPM affiché |
| RPM accélération | Suivi linéaire sans décrochage |
| Double comptage | Absent (RPM stable) |
| Température R1 (30min) | < 60°C |

## ⚠️ Avertissements

> **HAUTE TENSION** — Ce circuit manipule des signaux jusqu'à -450V. Respectez les précautions de sécurité.

> **AUTOMOBILE** — Installation par personne qualifiée uniquement.

## 📖 Documentation

📄 [Documentation technique complète](docs/Objectif_global_circuit_RPM.md)

## 👤 Auteur

**Mehdi** — [@mmmprod](https://github.com/mmmprod)

## 📄 License

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

**Version:** V5.4.12.1 | **Date:** Décembre 2025 | **Statut:** ✅ VALIDÉ — PRÊT FABRICATION