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

## Functional block map

| Functional block | What the reference documents | PawPlate implication | Evidence gap / status |
|---|---|---|---|
| AC input and EMI/EMC filter | The inverter board integrates the AC input and EMI/EMC filter. The guide describes a 220–240 V AC input range. | Treat inlet, protective devices, filtering, enclosure bonding and creepage/clearance as one coordinated appliance-level design area, not a generic connector module. | **Architecture documented; PawPlate implementation unresolved.** No build-ready mains layout or component values inferred here. |
| Rectifier and DC link | A bridge rectifier converts AC to a DC bus; the guide discusses DC-bus sensing. The bus is in the approximate 310–340 V peak range depending on input. | Both zones must be budgeted against the shared input and DC-link architecture. DC-bus voltage measurement and energy-storage fault behaviour need explicit treatment. | **Function documented; exact reusable parts/ratings not established by this map.** |
| Current sensing and pan detection | The current sensor supports pan detection, coil-current measurement and overcurrent detection. The guide's detailed component list prints `TLI4971-A120T5-U-F0001`, while another section names an A075 variant. However, Infineon's current [product table](https://www.infineon.com/product-table/current-sensors) and [2025–2026 selection guide](https://www.infineon.com/assets/row/public/documents/24/66/infineon-xensiv-sensor-solutions-productselectionguide-en.pdf) list A120 variants with `-U-E0001` and `-E0001` suffixes, not `-U-F0001`; the guide string may be a typo. | Sensor selection must follow range, bandwidth, isolation, fault-output behaviour and actual current waveform—not just nominal amperage. | **Ordering code unresolved.** Verify against the exact schematic/BOM revision before any shortlist; distributor listings for one variant do not establish availability of another. |
| Resonant tank and work coil | Each channel excites a resonant circuit made from its induction coil and resonant capacitor. The guide explains operation above the tank's resonant frequency to keep the load in the inductive region. | Continue treating coil and inverter as a paired choice. Use coil characterization under cookware, frequency and temperature conditions to bound the tank study. | **Critical gap:** the guide-level description is not enough to approve a coil, capacitor, operating envelope or layout for PawPlate. |
| Coil temperature sensing | The board exposes induction-hob coil-surface temperature sensor connectors; the guide's architecture also shows NTC inputs. | Include sensor placement, thermal coupling, connector/interface, open/short detection and shutdown policy in the replaceable coil/mechanical assembly specification. | **Functional interface documented; exact sensor implementation and thresholds need full guide/schematic review and validation.** |
| Current sensing and pan detection | The current sensor supports pan detection, coil-current measurement and overcurrent detection. The guide's detailed component list identifies TLI4971-A120T5-U-F0001, while another section names an A075 variant; this is a source inconsistency that must be resolved against the exact schematic/revision. | Sensor selection must follow range, bandwidth, isolation, fault-output behaviour and actual current waveform—not just nominal amperage. | **Reference identity inconsistent in the guide.** Existing distributor survey records the A120/A075 mismatch; neither is approved as PawPlate's sensor. |
| DC-bus and other feedback | The documented control architecture includes DC BUS SENSE and protection/control signals including OCP, OVP and OTP. The hardware overview shows a voltage sensor connected to the DC bus and ADC inputs for several measurements. | Define signal conditioning, fault thresholds, tolerances, sensor-failure response and isolation domains per signal. | **Signal roles documented; thresholds, tolerances and independent safety behaviour remain open.** |
| Auxiliary power | The reference uses two ICE5QR1680BG CoolSET flyback controllers and rails including 24 V for fan, isolated 5 V for HMI/connectivity, 3.3 V for controller circuitry and 19 V for gate drive; TLF1963TE and IFX25001ME are listed as LDOs. | Partition rails by load and isolation domain. A single low-voltage rail should not be assumed to satisfy UI, controller, fan and gate-drive requirements. | **Reference parts/rail roles identified; no PawPlate supply design or availability approval.** |
| MCU and control | CY8C6244AZI-S4D82 PSoC 6 runs inverter control on the reference board. The guide describes DC bus sense and OCP/OVP/OTP/detection inputs. | PawPlate's ESP32-family requirement remains in force. Compare the selected ESP32 variant's deterministic PWM, ADC triggering, fault inputs, startup defaults and failure response against these functional needs. | **Controller architecture documented; ESP32 variant and control partition unresolved.** Do not port reference firmware assumptions blindly. |
| Isolation and HMI interface | A 4DIR2401H digital isolator is listed; the board has an isolated supply and UART interface to the HMI/system-control board. | Define a documented interface between power-control and UI/supervisory modules, including reset, loss-of-communication and isolation failure states. | **Boundary documented; PawPlate interface contract not yet specified.** |
| Cooling and thermal system | The board has a fan and heatsink; the guide identifies a 24 V fan rail. | Thermal design includes semiconductor junctions, magnetic parts, capacitors, PCB, coil, glass and enclosure—not just the IGBT heatsink. | **Visible reference implementation only; PawPlate thermal model and abnormal-operation validation absent.** |
| Power rating and sharing | Infineon page advertises 3.3 kW output in its product data and describes 3.5 kW support in features. The user guide describes a 220–240 V input. | Compare ratings only after distinguishing mains input power from delivered heating/output power and confirming rating conditions. PawPlate's target remains 3.0 kW continuous total appliance input, with 3.68 kW as a provisional 230 V × 16 A ceiling. | **Not equivalent ratings.** The reference does not prove PawPlate meets its input-power target; both zones and auxiliaries must be included in PawPlate's budget. |

## What is public versus not established

### Publicly documented

- User guide and high-level functional architecture.
- Component names for key inverter, current-sensor, MCU and auxiliary-supply blocks.
- PDF schematic/layout download pages for the inverter board.
- A public Infineon community response states that the available board layout and schematic files are PDF-only; editable Gerber/CAD files were not available in that response.

### Not established by the public documentation reviewed here

- A complete, independently validated PawPlate BOM.
- Permission or ability to reuse reference-design firmware as-is for the ESP32 architecture.
- Exact current distributor availability for every reference component.
- A validated coil/resonant-capacitor pair for PawPlate.
- Complete safety case, compliance evidence, production files or a construction-ready layout.
- Proof that the reference's published output-power figure is comparable to PawPlate's total appliance-input target.

## Architecture conclusions

1. **Use the reference as a functional partitioning reference, not a copy-and-build plan.** It provides a concrete two-zone half-bridge architecture and identifies the major subsystem boundaries.
2. **Keep the coil and tank as an unresolved matched subsystem.** A named IGBT and driver do not make a compatible inverter without a characterized load, resonant capacitor, sensing and control strategy.
3. **Resolve the sensor ordering code before any parts shortlist.** The guide uses both A120 and A075 names, and the detailed-list suffix `-U-F0001` is not corroborated by Infineon's current product table, which lists `-U-E0001` / `-E0001` variants. Check the exact schematic/BOM revision; do not procure from the guide string alone.
4. **ESP32 remains the project controller-family decision.** Evaluate exact ESP32 variants against deterministic control needs. MCU hardware PWM fault functions can be useful but do not replace an independently engineered hardware inhibit/shutdown path.
5. **Treat the mains/DC-link, EMI, isolation, thermal design and protection as appliance-level engineering.** They cannot be safely modularized into assumed plug-and-play blocks without defined interfaces and validation.
6. **Keep the shared power budget explicit.** The scheduler must limit total appliance input across both zones and auxiliary loads; do not use the reference's output-power headline as the PawPlate input-power target.

## Next research gates

- Review the exact PDF schematic pages to reconcile current-sensor part number and trace the sensing/protection boundaries at a block level.
- Find a reference-supported coil and its matching resonant-network documentation, or continue the OEM coil route and request missing electrical data.
- Compare ESP32 variants only against a written timing/ADC/fault-response requirements table.
- Extend the sourcing table with lifecycle and distributor evidence only after exact part-number identity is confirmed.
- Keep all physical power-stage work behind qualified design review, safe test facilities and a separate appliance-safety validation plan.

This document is research-only. It does not contain circuit values, PCB layout instructions, or a build-ready mains design.
