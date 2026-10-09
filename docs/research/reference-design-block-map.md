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
- Espressif, [ESP32-S3 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-C6 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-P4 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/mcpwm/mcpwm_etm.html), and the [ESP32-family variant timing screen](esp32-variant-control-timing.md)

## Functional block map

| Functional block | What the reference documents | PawPlate implication | Evidence gap / status |
|---|---|---|---|
| AC input and EMI/EMC filter | The inverter board integrates the AC input and EMI/EMC filter. The guide describes a 220–240 V AC input range. | Treat inlet, protective devices, filtering, enclosure bonding and creepage/clearance as one coordinated appliance-level design area, not a generic connector module. | **Architecture documented; PawPlate implementation unresolved.** No build-ready mains layout or component values inferred here. |
| Rectifier and DC link | A bridge rectifier converts AC to a DC bus; the guide discusses DC-bus sensing. The bus is in the approximate 310–340 V peak range depending on input. | Both zones must be budgeted against the shared input and DC-link architecture. DC-bus voltage measurement and energy-storage fault behaviour need explicit treatment. | **Function documented; exact reusable parts/ratings not established by this map.** |
| Two-zone inverter | Two half-bridge channels; four IHW40N65R6 reverse-conducting 650 V / 40 A IGBTs total (two per coil); two 2ED21824S06J half-bridge drivers (one per coil). Infineon currently lists the exact IHW40N65R6 as active/preferred and in stock; the exact driver is listed active/preferred. | Strong reference for the leading half-bridge series-resonant candidate. Preserve switch/driver/tank as a matched design, not a parts-shopping exercise. | **Manufacturer lifecycle/sourcing signals are encouraging, not a validated BOM.** Verify live supply and do not infer PawPlate suitability from the part ratings alone. |
| Resonant tank and work coil | Each channel excites a resonant circuit made from its induction coil and resonant capacitor. The guide explains operation above the tank's resonant frequency to keep the load in the inductive region. | Continue treating coil and inverter as a paired choice. Use coil characterization under cookware, frequency and temperature conditions to bound the tank study. | **Critical gap:** the guide-level description is not enough to approve a coil, capacitor, operating envelope or layout for PawPlate. |
| Coil temperature sensing | The board exposes induction-hob coil-surface temperature sensor connectors; the guide's architecture also shows NTC inputs. | Include sensor placement, thermal coupling, connector/interface, open/short detection and shutdown policy in the replaceable coil/mechanical assembly specification. | **Functional interface documented; exact sensor implementation and thresholds need full guide/schematic review and validation.** |
| Current sensing and pan detection | The sensor provides analog current measurement and two fast overcurrent-detection outputs. Publicly indexed copies of the user guide disagree: one detailed component list prints `TLI4971-A120T5-U-E0001`, another prints `TLI4971-A120T5-U-F0001`, and a separate section refers to an A075 variant. Infineon's current [TLI4971-A120T5-U-E0001 product page](https://www.infineon.com/part/TLI4971-A120T5-U-E0001) lists it as active/preferred and in stock, with a 120 A range, typical 240 kHz bandwidth, diagnosis features and planned availability until at least 2038. | This current sensor family is a credible sourcing lead for further analysis, but its measurement range, thresholds, bandwidth, package, layout and fault response must match the actual channel. | **Reference BOM identity remains unresolved across guide copies.** The active A120 part is not proof that it is the exact device fitted to the reference board. Check the exact schematic/BOM revision before any purchase or substitution. |
| DC-bus and other feedback | The documented control architecture includes DC BUS SENSE and protection/control signals including OCP, OVP and OTP. The hardware overview shows a voltage sensor connected to the DC bus and ADC inputs for several measurements. | Define signal conditioning, fault thresholds, tolerances, sensor-failure response and isolation domains per signal. | **Signal roles documented; thresholds, tolerances and independent safety behaviour remain open.** |
| Auxiliary power | The reference uses two ICE5QR1680BG CoolSET flyback controllers and rails including 24 V for fan, isolated 5 V for HMI/connectivity, 3.3 V for controller circuitry and 19 V for gate drive; TLF1963TE and IFX25001ME are listed as LDOs. | Partition rails by load and isolation domain. A single low-voltage rail should not be assumed to satisfy UI, controller, fan and gate-drive requirements. | **Reference parts/rail roles identified; no PawPlate supply design or availability approval.** |
| MCU and control | CY8C6244AZI-S4D82 PSoC 6 runs inverter control on the reference board. The guide describes DC bus sense and OCP/OVP/OTP/detection inputs. | PawPlate's ESP32-family requirement remains in force. The exact ESP32 must be assessed against deterministic PWM, sampling, fault handling, startup defaults and failure response. | **Controller architecture documented; ESP32 variant and control partition unresolved.** Do not port reference firmware assumptions blindly. |
| Isolation and HMI interface | A 4DIR2401H digital isolator is listed; the board has an isolated supply and UART interface to the HMI/system-control board. | Define a documented interface between power-control and UI/supervisory modules, including reset, loss-of-communication and isolation failure states. | **Boundary documented; PawPlate interface contract not yet specified.** |
| Cooling and thermal system | The board has a fan and heatsink; the guide identifies a 24 V fan rail. | Thermal design includes semiconductor junctions, magnetic parts, capacitors, PCB, coil, glass and enclosure—not just the IGBT heatsink. | **Visible reference implementation only; PawPlate thermal model and abnormal-operation validation absent.** |
| Power rating and sharing | The user guide states a maximum 3,600 W system input at 220 V AC, with the bigger zone listed at 2,200 W (3,000 W boost) and the smaller at 1,400 W (2,000 W boost); listed coil inductances are 51 µH and 71 µH. The product page separately advertises 3.3 kW Pout and 3.5 kW support. | Use the manual's explicit input-power statement as the closest comparison to PawPlate's total-input target, while keeping zone ratings distinct from combined mains input and confirming test conditions. PawPlate's target remains 3.0 kW continuous total appliance input, with 3.68 kW as a provisional 230 V × 16 A ceiling. | **Published ratings use different labels and conditions.** The 3.3 kW Pout / 3.5 kW product-page figures should not be equated to 3.6 kW system input; PawPlate must include both zones and auxiliary loads in its own budget. |

## ESP32-family feasibility: what is and is not established

PawPlate has selected the **ESP32 family**, not a particular chip. Neither the exact variant nor the partition between supervisory and fast resonant control has been decided.

| Requirement | Official-documentation finding | Research conclusion |
|---|---|---|
| PWM at tens of kHz | Multiple ESP32 variants provide MCPWM waveform generation, complementary outputs, dead time and fault-state actions, but resource counts differ. | Feasibility must be checked against the exact topology, per-zone timer/operator needs, clock/resource sharing and selected silicon revision. |
| PWM-synchronous sampling | ESP32-S3 explicitly lacks MCPWM ETM events. ESP32-C6 and ESP32-P4 document MCPWM ETM support; P4 also documents a dedicated event comparator. | C6/P4 are candidates for further investigation, not selected variants. The ADC trigger API, acquisition mode, event-to-sample semantics and timing still need proof. |
| ADC acquisition and errata | ADCs and supported sampling modes vary by chip. ESP32-C6 has published ADC/GDMA errata on older silicon revisions; ADC-305 is documented as fixed in v0.2. | Check exact chip revision, errata/workarounds, ADC throughput, noise, calibration, DMA behaviour and any ETM route before comparing variants. |
| Hardware response to a fault | MCPWM fault features can force configured output states, but are MCU peripheral features rather than an independent appliance safety system. | External protection must inhibit switching even if the MCU is wedged, unpowered or misconfigured. Include fault-input pulse/electrical behaviour in the verification plan. |
| Safety-critical control | The cited ESP-IDF documentation does not establish a complete appliance-safety architecture for any variant. | A supervisory-only ESP32 role is one candidate partition; a fast-control role remains unproven until timing and fault response are validated. No MCU may be the sole protective layer. |

Sources: [ESP32-S3 MCPWM ETM](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-C6 MCPWM ETM](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-P4 MCPWM ETM](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-C6 errata](https://documentation.espressif.com/esp-chip-errata/en/latest/esp32c6/index.html). Detailed comparison and open verification gates are in `docs/research/esp32-variant-control-timing.md`.

## What is public versus not established

### Publicly documented

- User guide and high-level functional architecture.
- Component names for key inverter, current-sensor, MCU and auxiliary-supply blocks.
- PDF schematic/layout download pages for the inverter board.
- Infineon's user guide explicitly warns that reference boards are functionally tested only under typical load conditions and are not qualified for safety, manufacturing or lifetime; they may not meet CE or similar requirements. Treat the board as a research reference, not a safe appliance or proof of compliance. [User guide](https://www.infineon.com/assets/row/public/documents/60/44/infineon-ug-2024-05-smart-induction-cooktop-usermanual-en.pdf)
- Infineon's [ModusToolbox Smart Induction Cooktop Pack](https://softwaretools.infineon.com/tools/com.ifx.tb.tool.modustoolboxpacksmartinductioncooktop) advertises hardware Gerber files, application firmware and board support packages, but access is login/request gated. This is a potential route to editable files, not confirmation that PawPlate has permission to reuse them or that access will be granted.
- A currently active/preferred manufacturer product page for `TLI4971-A120T5-U-E0001`; this confirms the part exists and is offered, not that it is the reference board's fitted variant.

### Not established by the public documentation reviewed here

- A complete, independently validated PawPlate BOM.
- Permission or ability to reuse reference-design firmware as-is for the ESP32 architecture.
- Exact current distributor availability for every reference component.
- A validated coil/resonant-capacitor pair for PawPlate.
- Guaranteed phase-synchronized ADC acquisition on any ESP32 variant for the intended resonant-control loop.
- Complete safety case, compliance evidence, production files or a construction-ready layout.
- Proof that the reference's published output-power figure is comparable to PawPlate's total appliance-input target.

## Architecture conclusions

1. **Use the reference as a functional partitioning reference, not a copy-and-build plan.** It provides a concrete two-zone half-bridge architecture and identifies the major subsystem boundaries.
2. **Keep the coil and tank as an unresolved matched subsystem.** A named IGBT and driver do not make a compatible inverter without a characterized load, resonant capacitor, sensing and control strategy.
3. **The current sensor is now a more actionable research lead, but not a confirmed reference BOM match.** Infineon currently lists A120T5-U-E0001; different publicly indexed guide copies show both `-U-E0001` and `-U-F0001`, while another section mentions an A075 variant. Confirm against the actual schematic/BOM revision.
4. **ESP32 family is selected; exact variant and control partition are not.** Official documentation distinguishes S3 (no MCPWM ETM events) from C6 and P4 (MCPWM ETM documented), but this does not decide the chip. ADC task/API support, two-zone resource allocation, silicon errata, sample timing and fault response require target-specific verification. A supervisory partition remains one option, not a settled requirement.
5. **Treat the mains/DC-link, EMI, isolation, thermal design and protection as appliance-level engineering.** They cannot be safely modularized into assumed plug-and-play blocks without defined interfaces and validation.
6. **Keep the shared power budget explicit.** The scheduler must limit total appliance input across both zones and auxiliary loads; do not use the reference's output-power headline as the PawPlate input-power target.

## Next research gates

- Obtain/review the exact schematic PDF pages to reconcile the current-sensor ordering code and trace the sensing/protection boundaries at a block level.
- Find a reference-supported coil and its matching resonant-network documentation, or continue the OEM coil route and request missing electrical data.
- Write an explicit controller requirements table for the selected ESP32 variant, including timer allocation, sampling latency/jitter, fault pulse behaviour, startup state and failure response.
- Extend sourcing records with lifecycle and distributor evidence only after exact part-number identity is confirmed.
- Keep all physical power-stage work behind qualified design review, safe test facilities and a separate appliance-safety validation plan.

This document is research-only. It does not contain circuit values, PCB layout instructions, or a build-ready mains design.


## Follow-up controller feasibility screen: ESP32 family, variant undecided

A separate official-documentation check adds information to the variant shortlist without selecting a chip:

- Espressif explicitly states that ESP32-S3 does not support MCPWM ETM events.
- ESP32-C6 and ESP32-P4 document MCPWM ETM support; the P4 documentation describes a dedicated event comparator that can mark an ADC sampling phase.
- C6 and P4 datasheets list ADC and MCPWM among ETM-capable peripherals. P4 documents two 12-bit SAR ADCs and continuous DMA; C6 documents a 12-bit SAR ADC and a 160 MHz high-performance RISC-V core plus a low-power core.
- The P4 technical reference manual lists ADC ETM tasks including ADC_TASK_START for HP ADC multi-channel sampling. However, the high-level ADC driver docs do not establish the supported public API and full driver-mode configuration for phase-locked acquisition. ADC_TASK_START may start a sampling run rather than yield exactly one conversion per PWM event.

Therefore, the exact ESP32 variant remains undecided. A datasheet feature list does not establish trigger-to-sample timing, jitter, analog accuracy, control-loop performance, safety or suitability of a final board. Evaluate exact silicon revisions, ESP-IDF support, peripheral allocation and measured timing before selection; retain independent hardware shutdown.

Sources: [ESP32-C6 MCPWM ETM](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-P4 MCPWM ETM](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/mcpwm/mcpwm_etm.html), [ESP32-C6 datasheet](https://documentation.espressif.com/esp32-c6_datasheet_en.html), [ESP32-P4 datasheet](https://documentation.espressif.com/esp32-p4_datasheet_en.html), [ESP32-P4 technical reference manual (pre-release)](https://documentation.espressif.com/esp32-p4_technical_reference_manual_en.pdf). Detailed gates are recorded in docs/research/esp32-variant-control-timing.md.
