# Coil and power-stage research: standardization and repairability

Date: 2026-10-09  
Status: research findings and design recommendations; no components approved for a production design.

## Executive recommendation

Design PawPlate around **replaceable modules and documented interfaces**, not a single large integrated power board. Use standard catalog parts inside each module, but do not claim the coil or inverter is interchangeable until electrical and thermal compatibility is demonstrated.

Recommended service boundaries:
1. **Zone coil cassette** — coil, ferrite, temperature sensor(s), mounting frame, keyed high-frequency connection and a separate sensor connector.
2. **One power channel per zone** — switch devices, gate driver, local resonant capacitor network and current/voltage sensing. Keep the high-current resonant loop physically compact.
3. **Shared mains/input module** — inlet/terminal block, protection, EMI filtering, rectification/DC link and discharge provisions, if the chosen topology uses a shared DC link.
4. **Auxiliary low-voltage supply module** — replaceable, appropriately certified/isolated module where feasible.
5. **Controller/UI modules** — ESP32 controller board and separately replaceable user-interface board.
6. **Cooling module** — replaceable standard fan(s), with accessible filter/ducting where practical.
7. **Wiring harnesses** — keyed, documented, temperature/current-rated connectors; no connector should be chosen solely because it fits.

The service manual for the LEX EVI 640F is an example of this kind of service decomposition: it lists separate 180 × 180 mm left/right induction coils with thermistors, a display board, fan, power board and EMC/power-supply board. This is evidence that modular service parts are practical, **not** evidence of a universal electrical coil standard. Sources:  
- https://www.manualslib.com/manual/1633781/Lex-Evi-640f.html  
- https://manualzz.com/doc/60715990/lex-evi-640f-service-manual?lang=ur

## 1. Coil assemblies: what is standardized?

### Findings

There is no broadly established, cross-manufacturer domestic-hob coil interface found in this survey that guarantees interchangeability by diameter, connector or advertised watts. Replacement coils are usually sold for specific appliance models. The coil's electrical behaviour depends on winding geometry, conductor construction, ferrite, installation spacing, cookware, operating frequency and the resonant capacitor network.

A commercial 180 mm / 2 kW Midea replacement coil is a sourcing lead, but its public listing does not provide enough electrical detail to approve it for PawPlate:
- https://fixpart.de/produkt/midea-17466000000118-induktionsplatte

Infineon's EVAL-IHW25N140R5L evaluation board is a stronger technical reference: the manufacturer describes a replaceable 170 mm coil for a single-ended induction-cooking evaluation platform. That demonstrates replaceability inside a documented system, not plug-and-play compatibility with another inverter:
- https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L

Industrial induction suppliers also offer custom coils and assemblies, but these are often matched to the application and converter rather than being universal domestic-hob spares:
- https://www.itg-induktion.de/en/induction-systems/assemblies-for-induction-systems

### What a reusable PawPlate coil interface should specify

Treat this as a **project-defined interface**, not an existing industry standard.

| Interface item | Record/standardize |
|---|---|
| Mechanical | Outer dimensions, mounting points, height, glass-to-coil spacing, ferrite footprint, service removal path |
| Resonant electrical | Inductance with stated measurement conditions, tolerance, winding resistance, operating frequency range, current/temperature limits and intended resonant network |
| Construction | Conductor type/size, turns or winding drawing, insulation system, ferrite part/material and arrangement |
| Temperature | Sensor type, nominal resistance/curve or part number, position, connector pinout, open/short detection behaviour |
| Connection | Keyed, touch-safe where applicable, appropriately rated high-frequency connection; sensor connector kept distinct |
| Documentation | Manufacturer, exact part number/revision, drawing, approved inverter configuration, test history and replacement instructions |

Do not make a replacement coil acceptable based on diameter or nominal wattage alone. Do not assume two visually similar coils share inductance or thermal limits.

### Practical coil strategy

- First choice: find a coil assembly with a drawing and useful electrical/thermal specifications, ideally supported by an evaluation board or service documentation.
- Second choice: select one specific replacement assembly and characterize it as a **fixed approved assembly** for the first prototype; do not advertise generic interchangeability.
- Only later: create a PawPlate coil cassette standard once at least two candidate assemblies have been measured and shown to fit the same electrical/mechanical envelope.
- Avoid making the coil itself a user-wound part in the initial design; reproducibility and insulation/thermal validation would be difficult.

## 2. Power stage: standard parts that already exist

The component categories are mature and available from multiple suppliers. What is *not* standardized is a universal drop-in power stage for a domestic two-zone hob.

### Publicly documented reference designs

**Infineon Smart Induction Cooktop reference design (2024)**  
Its documentation identifies a bridge rectifier, TLI4971-A075T5-E0001 current sensor, IHW40N65R6 IGBTs, 2ED21824S06J gate driver and a coil/resonant-capacitor network. This is a concrete component-level reference, but it uses a PSoC MCU and should be treated as a design reference rather than an ESP32-compatible finished module.  
- Guide: https://www.infineon.com/dgdl/Infineon-UG-2024-05-Smart-Induction-Cooktop-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8e7ead30018e85ee55c8191a

**Infineon EVAL_2KW_SiC_IH (application note, 2025)**  
Documents a 2 kW half-bridge resonant induction-heating evaluation board using IMW65R020M2H 650 V SiC MOSFETs and 2ED21824S06J / 2ED21844S06J gate-driver IC options. The board includes a test coil and supports evaluation of different coil/load variants. The manufacturer explicitly says the evaluation PCB and auxiliary circuits are not optimized for a final customer design.  
- Application note: https://www.infineon.com/assets/row/public/documents/24/42/infineon-eval-2kw-sic-ih-applicationnotes-en.pdf

The references show that **switches, drivers, current sensors, rectifiers and capacitors can be selected as standard catalog components**. They do not establish that a board designed for one topology can be reused unchanged with a different coil, control loop or power rating.

### Standardizable power-stage categories

| Module/part | Standardization opportunity | Repairability recommendation |
|---|---|---|
| Power switches | Standard IGBT or SiC MOSFET product families | Use documented part numbers and accessible mounting; provide thermal interface access. Match device, topology and resonant tank together. |
| Gate driver | Standard driver IC families; sometimes a small separate driver board | Keep driver and fast hardware fault path inspectable; do not make firmware the only shutdown mechanism. |
| Resonant capacitors | Catalog polypropylene film capacitors designed for resonant/high-frequency current | Use replaceable parts with published RMS current, loss, temperature and pulse ratings. Avoid selecting from capacitance/voltage alone. |
| Rectifier | Standard bridge rectifier families | Make it replaceable and provide suitable heatsinking/insulation. Verify surge and thermal requirements. |
| DC-link capacitors | Standard electrolytic and/or film capacitor families, selected by circuit role | Ensure replacement access and document capacitance, voltage, ripple current, temperature and lifetime. |
| Current sensor | Catalog current-sensor ICs or suitable transducers | Make it replaceable where practical; define bandwidth, isolation, response and calibration. Independent fast overcurrent shutdown remains essential. |
| Voltage sensing | Standard rated resistors/amplifiers or isolated sensing components | Document voltage domain, ratings, creepage/clearance and calibration. |
| Temperature sensors | Standard NTCs and other sensors | Use a documented curve/part number and connector; detect open/short faults. |
| Cooling | Standard fans and heatsink materials | Prefer replaceable fan models and screw-mounted heatsinks; record airflow, temperature limits and service access. |
| Connectors/wiring | Catalog connector and wire families | Standardize keyed pinouts and ratings at the project level. High-frequency coil connections need special attention to current, temperature, insulation and parasitic inductance. |

Capacitor examples establish that specialized catalog components exist:
- TDK describes film-capacitor families for resonant circuits, snubbers and high-frequency power electronics: https://www.tdk-electronics.tdk.com/en/3191426
- A catalog listing for MULTICOMP PRO MP004055 describes a 3.3 µF polypropylene capacitor specifically for induction heating/resonant power supplies. Its high cost and application-specific rating are a reminder that a generic 3.3 µF part is not a substitute: https://de.farnell.com/en-DE/multicomp-pro/mp004055/cap-3-3uf-750vrms-film/dp/3463228

These are examples, **not a recommended PawPlate bill of materials**. The capacitor's actual voltage waveform, RMS current, frequency, loss, mounting and temperature must match the selected resonant circuit.

## 3. Topology affects repairability

Two candidates remain open:

- **Single-switch quasi-resonant:** fewer main switches, but high voltage/current stress and a coil/tank/control combination that must be carefully matched.
- **Half-bridge series-resonant:** two controlled switches and a more explicit half-bridge driver arrangement; public reference designs make it easier to study using documented parts.

Do not choose solely by parts count. Compare available schematics, documented fault handling, replacement part availability, thermal design, firmware/control complexity and measurement access.

A sensible modular boundary is one independently serviceable inverter channel per zone, while deciding separately whether rectification/DC-link resources should be shared. The resonant loop itself should remain physically compact; modularity must not introduce long, high-inductance wiring into the switching loop.

## 4. Repairability requirements to adopt now

1. **No potted power electronics** where this can be avoided; use fasteners and serviceable boards.
2. **No proprietary unlabelled harnesses**: publish connector series, pinout, wire rating and harness drawing.
3. **Separate low-voltage control/UI from hazardous power boards** and document isolation boundaries.
4. **Replaceable coil cassette** with a separate temperature-sensor interface.
5. **Replaceable fan, UI board, controller board, input module and zone power channel**.
6. **Published BOM with manufacturer part numbers**, acceptable alternates, footprint/thermal constraints and last-verified status.
7. **Versioned interfaces**: document which coil, power board, firmware and resonant network revisions are compatible.
8. **Diagnostic access**: fault codes, sensor values, event history and test points, without exposing unsafe live measurements to users.
9. **Independent hardware shutdown** for fast faults; the ESP32 supervises and logs but is not the only safety path.
10. **Repair documentation**: exploded view, disassembly sequence, wiring diagram, torque/thermal-interface notes, inspection and post-repair verification procedure.
11. **Long-lived standard parts**: prefer catalog components with multiple authorized distributors and public datasheets over undocumented no-name modules.
12. **Do not promise field repair before validation**: mains and resonant power stages require qualified design review and post-repair safety checks.

The relevant household-appliance safety standard is IEC 60335-2-6:2024, which explicitly covers stationary induction hobs. Modular design does not remove the need to assess the complete appliance against applicable safety requirements:
- https://webstore.iec.ch/en/publication/65424

## 5. Proposed next decisions

1. Compare the two inverter topologies using the Infineon reference material.
2. Decide whether each zone gets its own complete resonant power channel.
3. Find coil candidates with drawings and actual electrical/thermal data; contact suppliers for missing specifications.
4. Define the **PawPlate coil cassette interface** before choosing connectors or fixing enclosure geometry.
5. Create a shortlist of standard power-stage components only after topology, bus arrangement and candidate coil requirements are better defined.

No appliance was disassembled and no parts were purchased or physically tested for this research.
