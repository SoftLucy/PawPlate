# Standard-component sourcing survey: coils and reusable electronics

Date: 2026-10-09

## Purpose

PawPlate's key early question is not yet "which circuit do we build?" but "which components and subassemblies already exist, can be documented, and can be sourced affordably?" This is a first-pass research inventory, not an approved bill of materials.

## Initial mains design envelope

For the initial design case, assume a conventional European/German 230 V single-phase household connection with a 16 A-rated socket/circuit, subject to confirmation of the actual installation and circuit loading.

- Nominal arithmetic ceiling: 230 V × 16 A = 3,680 W input.
- This is not a universal guarantee that any socket/circuit can supply 3.68 kW continuously; wiring, protective devices, other circuit loads, connection condition and applicable installation rules matter.
- Use a shared appliance power budget so the sum of both zones plus auxiliary loads remains within the selected input limit.
- Keep a target in the roughly 3.4 kW class as a comparison point for the existing domino hob, but verify its nameplate/manual before treating it as a formal requirement.

References:
- DIN/VDE socket-outlet requirements: https://www.dke.de/de/normen-standards/dokument?id=7149275&type=dke%7Cdokument
- VDE consumer guidance on rated power of multiway sockets and overload hazards: https://www.vde.com/topics-en/consumer-protection/electrical-appliances

## Coil and coil-assembly candidates

### 1. Infineon EVAL-IHW25N140R5L — best documented technical reference found

https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L

Manufacturer page describes a single-ended induction-cooking evaluation board with up to 2 kW output and a replaceable 170 mm coil. This is valuable proof that a cooking coil can be a replaceable assembly in a documented power platform. It is not a two-zone design or a ready-to-use appliance, and it does not establish that its coil is compatible with a future PawPlate inverter.

### 2. Midea 17466000000118 — commercial replacement coil candidate

https://fixpart.de/produkt/midea-17466000000118-induktionsplatte

Replacement-parts listing describes an 180 mm, 2,000 W induction coil. The listing is useful as evidence of a commercially available part, but the available public product summary does not establish the full electrical interface (e.g. inductance/tolerance, winding construction, ferrite layout, thermal limits, or intended resonant circuit). Do not approve based on diameter and nominal wattage alone.

### 3. Youlong Z303060018 — commercial replacement coil candidate

https://fixpart.de/produkt/youlong-z303060018-induktionsplatte

Listing describes an 180 mm induction coil for specific PKM hob models. Again, compatibility with those original appliances is not a general electrical specification for a new inverter.

### 4. Industrial coil suppliers

https://www.itg-induktion.de/en/induction-systems/assemblies-for-induction-systems

Industrial supplier describes individually adapted coils, often made from shaped copper tubing and matched to the heating task/converter. This is useful evidence that industrial coils are frequently application-specific, and may be less appropriate than a domestic cooktop replacement assembly for PawPlate's cost target.

## Why coil interchangeability is difficult

A coil is not interchangeable solely by diameter or advertised watts. Important parameters include:
- Inductance and its measurement conditions/tolerance
- Winding geometry, turns and conductor construction (including high-frequency AC losses)
- Ferrite arrangement and backing
- Coil-to-glass spacing, mounting and thermal limits
- Resonant capacitor and intended inverter topology/frequency
- Current waveform, cookware interaction and pan-detection method
- Lead/connector arrangement and insulation/temperature ratings

The resonant network, switching devices, control algorithm, coil and cookware form one coupled system. A coil listing is a sourcing lead, not proof of compatibility. Manufacturer documentation and controlled characterization would be needed before choosing one.

## Standard electronic parts: likely reusable categories

These are established component categories with broad supplier ecosystems, but each part still needs electrical, thermal, lifetime, safety and availability review:

| Category | Likely sourcing route | Important qualification |
|---|---|---|
| ESP32 module | Espressif module ecosystem and authorized distributors | Exact variant must meet PWM/timing/ADC needs and lifecycle requirements |
| Gate driver | Semiconductor manufacturers/distributors | Must match topology, switch type, bus voltage, switching speed, isolation and fault handling |
| IGBT/MOSFET | Authorized semiconductor distributors | Select for resonant switching application; generic voltage/current ratings alone are insufficient |
| Resonant capacitors | Film-capacitor manufacturers/distributors | Must be suitable for high-frequency resonant current, pulse stress, temperature and lifetime |
| Current sensing | Current-sensor ICs or suitable transducers | Bandwidth, isolation, response time and fault path matter |
| Voltage sensing | Rated divider/isolated sensing solution | Creepage, clearance, transients, accuracy and isolation domain matter |
| Temperature sensing | NTCs, temperature ICs or thermocouples as suitable | Placement, response time, failure detection and thermal limits matter |
| Rectifier / DC-link parts | Authorized power-component suppliers | Surge/current ratings, inrush, ripple current, discharge and enclosure safety matter |
| Auxiliary supply | Certified isolated supply or reviewed custom design | Required isolation domains and rail needs depend on architecture |
| UI parts | Standard touch controller, display, LEDs/buttons/buzzer | Must remain separated from power-stage safety functions |
| Connectors / wire | Rated connector and cable families | Current, temperature, insulation, touch protection, retention and serviceability |

Infineon's cooking reference design specifically identifies a bridge rectifier, current sensor, IGBTs, gate driver and an LC resonant network; this corroborates that these are real component categories used in the application. See: https://www.infineon.com/dgdl/Infineon-UG-2024-05-Smart-Induction-Cooktop-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8e7ead30018e85ee55c8191a

## Initial recommendation

Prioritize sourcing and documentation in this order:

1. **Coil assemblies:** collect dimensional drawings, photos, manufacturer/model IDs, ferrite details, datasheets and any inductance/thermal data. Contact manufacturers where data is missing.
2. **Complete induction evaluation platforms:** identify designs with accessible schematics/BOM and replaceable coils; reuse architectural knowledge rather than assuming direct compatibility.
3. **Power-stage component families:** shortlist components only after comparing candidate inverter topologies and their requirements.
4. **Controller and UI:** standard modules are likely the easiest parts to source; settle the exact ESP32 only after timing and protection requirements are mapped.
5. **Mechanical interfaces:** define the coil mount, glass spacing and cooling arrangement after selecting candidate coil assemblies.

No part in this survey is yet approved for a PawPlate BOM. No physical testing or appliance disassembly was performed.
