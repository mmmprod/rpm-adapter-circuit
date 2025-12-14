# GLOBAL OBJECTIVE — RPM ADAPTER CIRCUIT

**Document version:** 2.0  
**Date:** 03 December 2025  
**Circuit reference:** V5.4.12.2  
**Status:** ✅ VALIDATED — READY FOR PRODUCTION  
**Author:** Mehdi

---

## CONTEXT FOR THE AI AND BUILDERS

### Target vehicle

| Parameter | Value |
|-----------|-------|
| Vehicle | Ford Mustang EcoBoost 2016 |
| Build | Stage 4+ (650+ HP on E85) |
| Coils | Granatelli Motor Sports high-performance |
| Coil voltage peaks | -300V to -450V (oscilloscope) |
| Gauge destination | Innovate MTX-D |

### Real-world environment

| Parameter | Value |
|-----------|-------|
| Country | France |
| Minimum temperature | -10°C (not -40°C) |
| Maximum temperature | +70°C |
| Module location | Cabin, center console |
| Environment | Protected, naturally ventilated |
| VTACH cable length | ~1m to Innovate |

### Design philosophy

| Principle | Application |
|----------|-------------|
| Fire-first | Layered protections, conservative margins |
| Galvanic isolation | H11L1 keeps the brutal engine domain away from logic |
| Oversizing | Generous ratings everywhere |
| Simplicity | 100% analog, no microcontroller |

---

## FUNCTIONAL GOAL

Capture the ignition signal from a coil-on-plug (negative command side), clean it, and deliver a safe, stable VTACH signal for the Innovate MTX-D.

- **Input:** Coil ignition signal -300V to -450V, steep edges, noisy EMI
- **Output:** Logic VTACH 0-5V, clean pulses, galvanically isolated
- **Protection:** No overheating, no fire risk, no way to destroy the Innovate

---

## GLOBAL ARCHITECTURE

```
[BATTERY 12V] → Fuse → D1 (1N5408 1000V) → TVS → π filter → PTC → R-78E → +5V
                                                                         ↓
[COIL -300V] → Ferrites → R1 47kΩ → TVS 250V → R2 1kΩ → Zener 5.1V → H11L1 → VTACH → Innovate
```

**Physical zones:**
- **Cabin:** Entire module (power + logic)
- **Engine bay:** Single shielded wire to the ignition coil

---

## RPM SIGNAL PATH — ENGINE SIDE

1. **Pickup:** Negative terminal of a coil-on-plug. Hostile waveform with -300V to -450V spikes and violent edges.
2. **Ferrites (FB1/FB2):** Two turns to shave off the fastest spikes before electronics see them.
3. **Current limiting R1:** 47kΩ 5W MOX (≥750V). Burns energy before it touches the rest of the design. Ratings: 750V > 450V (1.67×), SOTV 1215V (2.7×), power margin 125×.
4. **High-voltage TVS D4:** 1.5KE250CA bidirectional 250V 1.5kW. Clamps brutal swings; placed <5cm from J2 with direct chassis return.
5. **Secondary limiter R2 + Zener D5:** R2 1kΩ 3W MOX + D5 1N4733A 5.1V. Keeps the signal inside a safe 0-5V window without negative kicks.
6. **Optocoupler H11L1:** R3 680Ω (LED current), U2 H11L1M, D6 1N4148 reverse LED guard. I_LED ≈ 5.7mA vs 1.6mA required → 3.6× margin. At this point all the ugly energy is burned or clamped; galvanic isolation shields Innovate completely.

---

## CLEAN LOGIC SHAPING

- **H11L1 output:** Powered from the regulated +5V rail, delivers crisp logic pulses on each ignition event.
- **VTACH conditioning:** R4 470Ω series, R5 6.8kΩ pull-up to +5V, C7 **15nF** X7R (HF filtering, prevents double counts), D7 SA5.0CA ESD TVS.
- **Rise time math:** t_rise = 6.8kΩ × 15nF = 102µs → f_max ≈ 4.9kHz → RPM_max ≈ 147,000 (>> 6,500 RPM target).
- **Result:** Square pulses ~0.6V to 5V, low noise, ESD protected, Innovate-ready.

---

## POWER AND FIRE PROTECTION

**Supply chain:**
1. **Fuse F1 (500mA Fast-blow):** Close to the 12V input, limits available energy.
2. **Diode D1 (1N5408 1000V):** Reverse-polarity and ISO 7637-2 Pulse 1 (-150V) protection with massive margin.
3. **TVS D2 (15KPA18CA):** Swallows transients and load-dump; V_clamp ≈ 29V to protect 50V capacitors.
4. **π filter:** C_pre (100nF) → L1 (10-22µH shielded) → C1/C2 (22µF 50V) to scrub alternator/injector/turbo noise.
5. **Flyback diode D_flyback (1N5822):** Across L1 to protect the PTC when the inductor trips.
6. **PTC RUEF050 (500mA):** Trips to high resistance during internal faults, throttling energy and preventing fire.
7. **Zener D3 (ZY27 27V 2W):** Redundant clamp ahead of the regulator.
8. **Regulator R-78E5.0-0.5:** 7-28V input to clean 5V/500mA output at ~90% efficiency.

**Fire-first philosophy:**
| Protection | Function |
|------------|----------|
| 500mA fuse | Limits max available energy |
| Series diode | Stops polarity mistakes and back-feed |
| 500mA PTC | Resettable internal breaker |
| R1 5W MOX | Burns spikes without charring the PCB |
| Multiple TVS diodes | Clamp surges without continuous conduction |
| Galvanic isolation | Engine chaos is isolated from the gauge |

---

## GROUNDING STRATEGY

- **Point A — Logic ground:** R-78E pin2, H11L1 pins 2/5, +5V capacitors, D5 anode, J3 ground.
- **Point B — Power ground:** TVS D2/D4 return, π-filter capacitors, J1 ground.
- **Single junction:** Point A → Point B → Vehicle chassis. Validation: ΔV(module-chassis) ≤ 0.10V DC.
- **VTACH cable:** ~1m shielded; shield tied **only on the Innovate side** to avoid ground loops and RF re-triggering.

---

## CRITICAL BOM SNAPSHOT

### Semiconductors
| Ref | Part | Value | Function |
|-----|------|-------|----------|
| U1 | R-78E5.0-0.5 | 5V 500mA | DC-DC regulator |
| U2 | H11L1M | Opto Schmitt | Galvanic isolation |
| D1 | 1N5408 | 1000V 3A | Reverse/ISO 7637-2 |
| D_flyback | 1N5822 | Schottky 40V 3A | L1 flyback |
| D2 | 15KPA18CA | TVS 18V 15kW | Load-dump guard |
| D3 | ZY27 (Diotec) | Zener 27V 2W | Regulator protection |
| D4 | 1.5KE250CA | TVS 250V 1.5kW | Coil protection |
| D5 | 1N4733A | Zener 5.1V 1W | Signal clamp |
| D6 | 1N4148 | 100V signal diode | LED reverse guard |
| D7 | SA5.0CA | 5V ESD TVS | Output protection |

### Resistors
| Ref | Value | Power | Type | Note |
|-----|-------|-------|------|------|
| R1 | 47kΩ | 5W | Metal Oxide | **V_rating ≥ 750V** |
| R2 | 1kΩ | 3W | Metal Oxide | — |
| R3 | 680Ω | 0.25W | Metal Film | LED current |
| R4 | 470Ω | 0.25W | Metal Film | Output series |
| R5 | 6.8kΩ | 0.25W | Metal Film | Pull-up |

### Capacitors
| Ref | Value | Voltage | Type |
|-----|-------|---------|------|
| C1, C2 | 22µF | 50V | Low-ESR electrolytic |
| C3, C4, C6, C_pre | 100nF | 50V | X7R ceramic |
| C5 | 22µF | 16V | Low-ESR electrolytic |
| C7 | **15nF** | 50V | X7R ceramic |

### Protections and passives
| Ref | Type | Specs |
|-----|------|-------|
| F1 | Glass 5×20mm | 500mA **Fast-blow** |
| PTC | RUEF050 | 500mA hold, 30V (Bourns) |
| L1 | Shielded inductor | 10-22µH |
| FB1, FB2 | Ferrite | Two turns ring |

---

## VALIDATED DESIGN CHOICES

- **R1 single 47kΩ 5W MOX:** 750V rating with 1.67× voltage margin and huge power headroom; no need for 2×22kΩ in series.
- **50V electrolytics:** TVS clamps at ~29V during load-dump, giving 1.72× voltage margin — 50V parts are sufficient.
- **R3 = 680Ω:** Delivers 3.6× LED current margin at -10°C (real climate). 470Ω only needed for arctic -40°C.
- **D3 27V Zener kept:** Redundant safety if D2 fails; costs pennies and aligns with fire-first philosophy.
- **C7 = 15nF:** Rise time 102µs (max ~147k RPM). 33nF works but 15nF gives cleaner speed while still filtering.

---

## MANDATORY VALIDATION TESTS

- **Static (power OFF):** Ground continuity, no VCC/GND short, diode forward drops 0.3-0.7V, D_flyback forward drop 0.4-0.6V with red on +12V_PROT and black on +12V_FILT.
- **Power tests:** +5V between 4.90-5.10V, ripple <50mV, idle current <10mA.
- **On-vehicle:** Idle ~700 RPM, linear tracking on acceleration, no double counting, R1 <60°C after 30 minutes.

---

## CABLING CHECKPOINTS

| # | Check | Importance |
|---|-------|------------|
| 1 | D4 returns to J2 pin2 (direct engine chassis) | Critical |
| 2 | VTACH shield connected only on Innovate side | Critical |
| 3 | R1 elevated ~3mm from PCB with airflow | Important |
| 4 | TVS D2 within <5cm of J1 terminal block | Important |
| 5 | Star ground respected (Point A → Point B → Chassis) | Important |

---

## WHAT THE CIRCUIT OUTPUTS

| Parameter | Value |
|-----------|-------|
| Signal type | Square pulses |
| High level | ~5V |
| Low level | ~0.6V |
| Sync | 1 pulse per ignition event |
