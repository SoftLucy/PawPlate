# Dual-zone reference-design functional-block map

**Research date:** 2026-10-09  
**Status:** source-backed architecture map; not a circuit design, validated BOM, or procurement approval.

## Purpose

Map the documented functional blocks of Infineon's REF-SHA3K3IHWR5SYS smart induction cooktop reference design against PawPlate's requirements. The reference is useful because it documents a real two-zone, half-bridge induction architecture, but its parts, board layout, power ratings, firmware and safety behaviour are not automatically transferable to PawPlate.

Primary sources:

- Infineon, [REF-SHA3K3IHWR5SYS product page](https://www.infineon.com/evaluation-board/REF-SHA3K3IHWR5SYS)
- Infineon, [Smart Induction Cooktop Reference Design user guide (PDF)](https://www.infineon.com/assets/row/public/documents/60/44/infineon-ug-2024-05-smart-induction-cooktop-usermanual-en.pdf)
- Infineon, [inverter-board schematics download page](https://www.infineon.com/gated/infineon-schematics-ref-sha3k3ihwr5sys-inverter-board-pcbdesigndata-en_736d2c96-b626-4eab-b929-b0211b226939)
- Infineon, [developer-community answer on available design files](https://community.infineon.com/t5/IGBT/REF-SHA3K3IHWR5SYS/td-p/793733)
- Infineon, [TLI4971-A120T5-U-E0001 product page](https://www.infineon.com/part/TLI4971-A120T5-U-E0001)
- Espressif, [ESP32-S3 MCPWM documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/mcpwm.html), [ADC continuous-mode documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/adc/adc_continuous.html), and [technical reference manual](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

## Functional block map

| Functional block | What the reference documents | PawPlate implication | Evidence gap / status |
|---|---|---|---|
| AC input and EMI/EMC filter | The inverter board integrates the AC input and EMI/EMC filter. The guide describes a 220–240 V AC input range. | Treat inlet, protective devices, filtering, enclosure bonding and creepage/clearance as one coordinated appliance-level design area, not a generic connector module. | **Architecture documented; PawPlate implementation unresolved.** No build-ready mains layout or component values inferred here. |
| Rectifier and DC link | A bridge rectifier converts AC to a DC bus; the guide discusses DC-bus sensing. The bus is in the approximate 310–340 V peak range depending on input. | Both zones must be budgeted against the shared input and DC-link architecture. DC-bus voltage measurement and energy-storage fault behaviour need explicit treatment. | **Function documented; exact reusable parts/ratings not established by this map.** |
| Two-zone inverter | Two half-bridge channels; four IHW40N65R6 reverse-conducting 650 V / 40 A IGBTs total (two per coil); two 2ED21824S06J half-bridge drivers (one per coil). | Strong reference for the leading half-bridge series-resonant candidate. Preserve switch/driver/tank as a matched design, not a parts-shopping exercise. | **Exact reference parts identified; PawPlate suitability and current sourcing not approved.** Do not substitute IGBTs based only on headline current. |
| Resonant tank and work coil | Each channel excites a resonant circuit made from its induction coil and resonant capacitor. The guide explains operation above the tank's resonant frequency to keep the load in the inductive region. | Continue treating coil and inverter as a paired choice. Use coil characterization under cookware, frequency and temperature conditions to bound the tank study. | **Critical gap:** the guide-level description is not enough to approve a coil, capacitor, operating envelope or layout for PawPlate. |
| Coil temperature sensing | The board exposes induction-hob coil-surface temperature sensor connectors; the guide's architecture also shows NTC inputs. | Include sensor placement, thermal coupling, connector/interface, open/short detection and shutdown policy in the replaceable coil/mechanical assembly specification. | **Functional interface documented; exact sensor implementation and thresholds need full guide/schematic review and validation.** |
| Current sensing and pan detection | The sensor provides analog current measurement and two fast overcurrent-detection outputs. The user guide's detailed list prints `TLI4971-A120T5-U-F0001`, while another section names an A075 variant. Infineon's current product page lists `TLI4971-A120T5-U-E0001` as active/preferred and in stock, with a 120 A range, typical 240 kHz bandwidth, diagnosis features and planned availability until at least 2038. | This current sensor family is a credible sourcing lead for further analysis, but its measurement range, thresholds, bandwidth, package, layout and fault response must match the actual channel. | **Reference BOM identity remains unresolved.** The currently listed A120 part is not proof that it is the exact device fitted to the reference board. Do not buy it as a drop-in replacement without checking the schematic/BOM revision. |
| DC-bus and other feedback | The documented control architecture includes DC BUS SENSE and protection/control signals including OCP, OVP and OTP. The hardware overview shows a voltage sensor connected to the DC bus and ADC inputs for several measurements. | Define signal conditioning, fault thresholds, tolerances, sensor-failure response and isolation domains per signal. | **Signal roles documented; thresholds, tolerances and independent safety behaviour remain open.** |
| Auxiliary power | The reference uses two ICE5QR1680BG CoolSET flyback controllers and rails including 24 V for fan, isolated 5 V for HMI/connectivity, 3.3 V for controller circuitry and 19 V for gate drive; TLF1963TE and IFX25001ME are listed as LDOs. | Partition rails by load and isolation domain. A single low-voltage rail should not be assumed to satisfy UI, controller, fan and gate-drive requirements. | **Reference parts/rail roles identified; no PawPlate supply design or availability approval.** |
| MCU and control | CY8C6244AZI-S4D82 PSoC 6 runs inverter control on the reference board. The guide describes DC bus sense and OCP/OVP/OTP/detection inputs. | PawPlate's ESP32-family requirement remains in force. The exact ESP32 must be assessed against deterministic PWM, sampling, fault handling, startup defaults and failure response. | **Controller architecture documented; ESP32 variant and control partition unresolved.** Do not port reference firmware assumptions blindly. |
| Isolation and HMI interface | A 4DIR2401H digital isolator is listed; the board has an isolated supply and UART interface to the HMI/system-control board. | Define a documented interface between power-control and UI/supervisory modules, including reset, loss-of-communication and isolation failure states. | **Boundary documented; PawPlate interface contract not yet specified.** |
| Cooling and thermal system | The board has a fan and heatsink; the guide identifies a 24 V fan rail. | Thermal design includes semiconductor junctions, magnetic parts, capacitors, PCB, coil, glass and enclosure—not just the IGBT heatsink. | **Visible reference implementation only; PawPlate thermal model and abnormal-operation validation absent.** |
| Power rating and sharing | Infineon page advertises 3.3 kW output in its product data and describes 3.5 kW support in features. The user guide describes a 220–240 V input. | Compare ratings only after distinguishing mains input power from delivered heating/output power and confirming rating conditions. PawPlate's target remains 3.0 kW continuous total appliance input, with 3.68 kW as a provisional 230 V × 16 A ceiling. | **Not equivalent ratings.** The reference does not prove PawPlate meets its input-power target; both zones and auxiliaries must be included in PawPlate's budget. |

## ESP32-S3 feasibility: what the documentation does and does not establish

The ESP32 family remains PawPlate's chosen controller family; this is a feasibility check, not a recommendation to lock the design to ESP32-S3.

| Requirement | Official documentation finding | Research conclusion |
|---|---|---|
| PWM at tens of kHz | ESP32-S3 MCPWM has timer, comparator, generator, complementary output and dead-time facilities. | Frequency generation is plausible in principle. Exact edge resolution, update behaviour and output assignment still need a target-specific proof of concept. |
| Two independently controlled zones | ESP32-S3 documents two independent MCPWM groups. | The raw group count is promising, but resource counts, clock sharing, pin routing and how each zone's switching waveform is formed must be checked against the chosen topology. |
| Hardware response to a fault | MCPWM can force generator outputs to configured states on a GPIO fault. The technical reference manual documents pulse-width constraints for reliable fault sampling. | This is useful supplementary protection, not a safety-rated or independent appliance shutdown. External protection must inhibit switching even if the MCU is wedged, unpowered or misconfigured. The fault input's minimum pulse and electrical behaviour must be included in the test plan. |
| Current and bus-voltage sampling | ADC continuous mode supports repeated conversions and DMA, but Espressif documents limitations, including ADC2 DMA issues on ESP32-S3 and operating-mode restrictions. The documentation reviewed does not establish a simple, guaranteed phase-locked ADC trigger from MCPWM suitable for a resonant-current control loop. | Do not assume that high sample throughput equals deterministic switching-synchronous sampling. Prove timing, jitter, channel sequencing, conversion latency, analog settling and Wi-Fi/ADC interactions—or assign fast inner-loop sensing/control to dedicated hardware. |
| Safety-critical control | ESP-IDF is a general embedded software platform; the cited peripheral documentation does not establish a complete appliance-safety architecture. | Use the ESP32 for supervisory control, UI, diagnostics and power scheduling unless a separately reviewed timing design demonstrates that any faster control tasks are appropriate. Do not make firmware the sole protective layer. |

Sources: [MCPWM](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/mcpwm.html), [ADC continuous mode](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/adc/adc_continuous.html), [ESP32-S3 technical reference manual](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf).

## What is public versus not established

### Publicly documented

- User guide and high-level functional architecture.
- Component names for key inverter, current-sensor, MCU and auxiliary-supply blocks.
- PDF schematic/layout download pages for the inverter board.
- A public Infineon community response states that the available board layout and schematic files are PDF-only; editable Gerber/CAD files were not available in that response.
- A currently active/preferred manufacturer product page for `TLI4971-A120T5-U-E0001`; this confirms the part exists and is offered, not that it is the reference board's fitted variant.

### Not established by the public documentation reviewed here

- A complete, independently validated PawPlate BOM.
- Permission or ability to reuse reference-design firmware as-is for the ESP32 architecture.
- Exact current distributor availability for every reference component.
- A validated coil/resonant-capacitor pair for PawPlate.
- Guaranteed phase-synchronized ESP32-S3 ADC acquisition for the intended resonant-control loop.
- Complete safety case, compliance evidence, production files or a construction-ready layout.
- Proof that the reference's published output-power figure is comparable to PawPlate's total appliance-input target.

## Architecture conclusions

1. **Use the reference as a functional partitioning reference, not a copy-and-build plan.** It provides a concrete two-zone half-bridge architecture and identifies the major subsystem boundaries.
2. **Keep the coil and tank as an unresolved matched subsystem.** A named IGBT and driver do not make a compatible inverter without a characterized load, resonant capacitor, sensing and control strategy.
3. **The current sensor is now a more actionable research lead, but not a confirmed reference BOM match.** The A120T5-U-E0001 is currently listed by Infineon; the reference guide's `-U-F0001` string and A075 mention still require schematic/BOM reconciliation.
4. **ESP32 remains the project controller-family decision.** ESP32-S3 MCPWM appears capable of generating useful PWM patterns, but synchronized ADC timing and fault-path assumptions are not yet proven for resonant inner-loop control. A supervisory-control partition is the conservative current assumption.
5. **Treat the mains/DC-link, EMI, isolation, thermal design and protection as appliance-level engineering.** They cannot be safely modularized into assumed plug-and-play blocks without defined interfaces and validation.
6. **Keep the shared power budget explicit.** The scheduler must limit total appliance input across both zones and auxiliary loads; do not use the reference's output-power headline as the PawPlate input-power target.

## Next research gates

- Obtain/review the exact schematic PDF pages to reconcile the current-sensor ordering code and trace the sensing/protection boundaries at a block level.
- Find a reference-supported coil and its matching resonant-network documentation, or continue the OEM coil route and request missing electrical data.
- Write an explicit controller requirements table for the selected ESP32 variant, including timer allocation, sampling latency/jitter, fault pulse behaviour, startup state and failure response.
- Extend sourcing records with lifecycle and distributor evidence only after exact part-number identity is confirmed.
- Keep all physical power-stage work behind qualified design review, safe test facilities and a separate appliance-safety validation plan.

This document is research-only. It does not contain circuit values, PCB layout instructions, or a build-ready mains design.
