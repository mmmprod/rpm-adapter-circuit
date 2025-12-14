# 🔌 RPM Adapter Circuit

![Version](https://img.shields.io/badge/Version-V5.4.12.4-blue)
![Status](https://img.shields.io/badge/Status-VALIDATED-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)
![ISO](https://img.shields.io/badge/ISO_7637--2-Compliant-green)

**Galvanically isolated RPM adapter for a Ford Mustang EcoBoost Stage 4+ (650+ HP).**

High-voltage coil signal conditioning for an Innovate MTX-D gauge with layered fire protection and zero microcontrollers.

---

## 🎯 Goal

Convert the harsh ignition signal from a coil-on-plug pencil coil (-300V to -450V) into a clean logic-level waveform (0-5V) for the Innovate MTX-D RPM gauge.

## 🚗 Target vehicle

| Parameter | Value |
|-----------|-------|
| Vehicle | Ford Mustang EcoBoost 2016 |
| Build | Stage 4+ (650+ HP on E85) |
| Coils | Granatelli Motor Sports high-performance |
| Gauge | Innovate MTX-D |

## ⚡ Core highlights

- ✅ **Galvanic isolation** via H11L1 optocoupler
- ✅ **Multi-layer fire safety** (vehicle fuse, PTC, TVS, Zener)
- ✅ **ISO 7637-2 ready** — Negative transients protection (-150V)
- ✅ **105°C automotive capacitors** — 20+ year lifetime
- ✅ **Mechanical reliability** — RTV silicone, ferrules, threadlocker
- ✅ **UL94 V-0 enclosure** — Auto-extinguishing, ventilated
- ✅ **100% analog** — no firmware to brick
- ✅ **Validated and tested** — production-ready layout

## 🆕 Changelog

### V5.4.12.4 (09 Dec 2025)
📋 **Safety documentation** (Lead Architect sign-off):
- **F_vehicle**: Mandatory time-delay fuse placement documented within <10cm of tap point
- **Enclosure**: New section with UL94 V-0 and ventilation requirements
- **Fuses**: Clarified Time-delay (vehicle) vs Fast-blow (PCB)

### V5.4.12.3 (09 Dec 2025)
🔴 **Mechanical reliability** (Gemini audit):
- **Capacitors**: 85°C → **105°C automotive** (10× lifetime)
- **D2/R1 mounting**: + Neutral RTV silicone (anti-vibration)
- **Terminal blocks**: + Ferrules + Threadlocker (anti-loosening)

### V5.4.12.2 (09 Dec 2025)
🔴 **ISO 7637-2 protection**:
- **D1**: 1N5822 (40V) → **1N5408 (1000V)** — 6.6× margin vs -150V pulse

## 🏗️ Architecture

```
+BATT → F_vehicle (1A) → 30cm harness → F1 (500mA) → D1 (1N5408) → TVS → π filter → R-78E → +5V
                                                                                         ↓
[COIL -300V] → Ferrites → R1 47kΩ → TVS 250V → R2 1kΩ → Zener 5.1V → H11L1 → VTACH → Innovate
```

## 📁 Project structure

```
rpm-adapter-circuit/
├── README.md                              # This file
├── LICENSE
├── docs/
│   ├── Objectif_global_circuit_RPM.md     # Project overview and design intent
│   ├── Circuit_RPM_V5_4_12_4.md           # Detailed circuit schematic V5.4.12.4
│   ├── PROTOCOLE_TEST_RPM_V2_7_3.md       # Full validation test protocol
│   ├── GUIDE_MONTAGE_V5_4_12_2.md         # Step-by-step build guide
│   └── VERIFICATION_REPORT.md             # Repo verification notes
└── bom/
    └── BOM_V5_4_12_4.md                   # Bill of materials V5.4.12.4
```

## 📖 Documentation

| Document | Description |
|----------|-------------|
| [Project Overview](docs/Objectif_global_circuit_RPM.md) | Design intent and philosophy |
| [Circuit V5.4.12.4](docs/Circuit_RPM_V5_4_12_4.md) | Schematic, blocks, topologies |
| [Build Guide](docs/GUIDE_MONTAGE_V5_4_12_2.md) | Step-by-step assembly |
| [Test Protocol V2.7.3](docs/PROTOCOLE_TEST_RPM_V2_7_3.md) | Lab + field tests, GO/NO-GO checklist |
| [BOM V5.4.12.4](bom/BOM_V5_4_12_4.md) | Complete component list |

## 📋 Critical components

| Ref | Component | Value | Function |
|-----|-----------|-------|----------|
| **F_vehicle** | **ATO blade** 🔴 | **1A Time-delay** | **Cable protection (MANDATORY)** |
| U1 | R-78E5.0-0.5 | 5V 500mA | DC-DC regulator |
| U2 | H11L1M | Opto Schmitt | Galvanic isolation |
| **D1** | **1N5408** 🔴 | **1000V 3A** | **ISO 7637-2 transient guard** |
| D2 | 15KPA18CA | TVS 18V 15kW | Load-dump protection |
| R1 | MOX resistor | 47kΩ 5W | High-voltage current limit |
| **C1, C2** | **Panasonic FR** 🔴 | **22µF 50V 105°C** | **Automotive filtering** |

## 🧪 Validation tests

| Test | GO criteria |
|------|-------------|
| Idle RPM | ~700 RPM displayed |
| Acceleration RPM | Linear tracking without dropouts |
| Double counting | Absent (stable RPM) |
| R1 temperature (30min) | < 60°C |
| D1 voltage drop | 0.6-0.7V (11.3V after D1) |

## ⚠️ Warnings

> **HIGH VOLTAGE** — Coil signals hit -450V. Follow safety procedures.

> **F_VEHICLE IS MANDATORY** — 1A fuse within 10cm of the tap point, otherwise fire risk on the harness.

> **D1 ORIENTATION CRITICAL** — Triple-check 1N5408 polarity before power-up.

> **105°C CAPACITORS ONLY** — Use automotive 105°C electrolytics exclusively.

> **UL94 V-0 ENCLOSURE** — Self-extinguishing plastic required, with ventilation.

## 🛡️ Compliance

- ✅ **ISO 16750-2** — Vehicle electrical power supply
- ✅ **ISO 7637-2** — Electrical transients (Pulse 1 to 5)
- ✅ **AEC-Q100 Grade 3** — Components -40°C to +85°C
- ✅ **UL94 V-0** — Self-extinguishing enclosure
- ✅ **Mechanical reliability** — RTV + ferrules + threadlocker

## 💰 Estimated cost

| Version | Circuit cost | Reliability |
|---------|--------------|-------------|
| V5.4.12.2 | €29-48 | Standard |
| **V5.4.12.4** | **€35-55** | **10× long-term** |

## 👤 Author

**Mehdi** — [@mmmprod](https://github.com/mmmprod)

## 📄 License

This project is licensed under MIT. See [LICENSE](LICENSE) for details.

---

**Version:** V5.4.12.4 | **Date:** December 2025 | **Status:** ✅ PRODUCTION-READY | **Confidence:** 10/10 🔥
