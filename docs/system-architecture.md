# System architecture (draft)

## Fixed project decisions

- **Main microcontroller: ESP32 family.** This is a project decision, not yet a final chip/module selection. Select the exact ESP32 variant only after checking timing resources, ADC needs, lifecycle, module availability, and firmware support.
- **Modular architecture:** define replaceable functional modules and documented electrical/mechanical interfaces.
- **Two cooking zones:** design around two independently monitored heating channels with a shared appliance-level power budget.
- **Initial mains envelope:** target 3.0 kW continuous total input, with 3.68 kW as the peak input ceiling (230 V × 16 A). This is a project design target, not a guarantee for every household circuit; the actual connection and installation must support the load. The appliance scheduler must enforce the total input limit, including both zones and auxiliary loads.
- **Research-first:** use published reference designs, simulations, and non-destructive observations before considering hardware experiments.

## Functional blocks

1. Mains input, disconnect/protection, filtering, and rectification/DC link
2. One resonant inverter channel per zone (topology still undecided)
3. Coil, resonant capacitor network, ferrite and mechanical mounting per zone
4. Gate-driver and switching module(s)
5. Voltage/current sensing, cookware detection, and fast hardware fault shutdown
6. Low-voltage auxiliary power supply and controller power
7. ESP32 controller board and firmware
8. User interface: touch/button inputs, display/indicators, buzzer as appropriate
9. Thermal sensing, airflow/cooling, enclosure, insulation and protective earthing as applicable
10. Shared power scheduler, diagnostics, fault log and service interface

## Proposed module boundaries

- **Mains/DC-link module:** input protection, EMI filtering, rectification and DC-link energy storage. High-voltage boundary; not a hobby plug-in module.
- **Zone power module (x2):** switching devices, gate driver, local resonant network and coil interface.
- **Sensor/protection module(s):** current/voltage and temperature conditioning, pan detection signals, interlocks and hardware fault line. Fast protection must not depend on ESP32 task scheduling.
- **ESP32 controller module:** user settings, power allocation, state machine, telemetry and supervisory control. MCPWM may be evaluated for timing generation, but exact suitability must be proven for the selected ESP32 and control topology.
- **UI module:** touch/buttons, display/LEDs and user feedback, separated from power electronics.
- **Mechanical/thermal module:** glass/ceramic top, coil supports, ferrite, heatsinks, fans/ducts, enclosure, shielding and service access.
- **Auxiliary supply module:** suitable isolated low-voltage rails for controller/UI and any required driver supplies; exact isolation and rail needs depend on the chosen power architecture.

## Interface principles

Prefer documented, keyed connectors and explicit interface specifications: pinout, voltage/current range, signal direction, isolation domain, fault-state behaviour, connector family, temperature rating, and revision. Do not standardize a connector before its electrical and safety requirements are known.

The ESP32 must not be the only means of turning off a dangerous power stage. Define a hardware-default-off enable path, gate-driver undervoltage behaviour, overcurrent shutdown, thermal shutdown and safe recovery policy. Exact implementations require qualified design and validation.

## Open decisions

- Exact ESP32 variant/module and timing/control partition
- Single shared DC link versus other power architecture
- Per-zone topology: half-bridge series-resonant is a candidate, not a decision
- Shared versus independent inverter/control boards
- Maximum total input power and zone power-sharing policy
- Sensor types, fault thresholds and hardware shutdown architecture
- Cooling, enclosure, insulation, earthing and EMC layout
- Connector/interface standards and component sourcing
- Applicable standards and verification plan

No topology has been selected and no design is ready for mains connection. Evaluate candidates using published references, simulation, cost estimates, and qualified engineering review before physical implementation.


## Research update (2026-10-09)

Half-bridge series-resonant is currently the leading topology candidate, based on the dual-zone references compared in `docs/research/coil-inverter-pairing-study.md`. It is not yet a fixed decision. Keep the coil and inverter as a paired design choice: the Infineon 170 mm coil kit is a useful 20-50 kHz matched-load reference, while Midea 17466000000118 is a low-cost 180 mm spare candidate with missing electrical data. Neither is approved for PawPlate. The next gate is to obtain or measure electrical data for an exact coil before setting resonant-network and operating limits.


## Research update: controller timing constraint (2026-10-09)

The ESP32 family remains the project controller-family decision, but do not assume every variant is suitable for resonant inner-loop control. Espressif's current ESP32-S3 MCPWM documentation explicitly states that MCPWM ETM events are not supported on ESP32-S3, and its continuous ADC interface does not expose a direct MCPWM event-trigger configuration. This is a meaningful limitation for deterministic switching-synchronous current sampling. Until a selected variant and timing test prove otherwise, treat the ESP32 as the supervisory controller for UI, diagnostics and total-input power scheduling; evaluate any fast control loop separately and retain independent hardware shutdown. See `docs/research/reference-design-block-map.md` for the source-backed comparison.
