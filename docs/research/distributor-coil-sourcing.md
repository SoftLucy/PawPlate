# Electronics-distributor coil and magnetic-component survey

**Survey date:** 2026-10-09  
**Scope:** Mouser Europe and Farnell Germany/Europe  
**Status:** initial search; not exhaustive and not a procurement approval.

## Result in brief

The distributor search did **not** identify a directly suitable, standard hob-sized 140–280 mm induction-cooking coil assembly with published cooking-power capability and the electrical/mechanical data needed to pair it to an inverter. These distributors are still useful for sourcing magnetic materials and small wireless-power coils, but those are different component classes. Do not add them to `data/coil-assemblies.csv` as candidate cooking coils unless a manufacturer explicitly supports the intended cooking application and the full coil assembly is characterized.

## Relevant sourced products and why they are not hob coils

| Product | Published details | Potential research value | Why it is not a cooking-coil candidate |
|---|---|---|---|
| Würth Elektronik 760308101, Farnell order 2114371 | Wireless-power receiver coil; 24 µH; max DCR 0.1 Ω; 53.3 × 53.3 × 6 mm; Q 90 at 125 kHz; stated saturation current 10 A. [Farnell](https://de.farnell.com/en-DE/wurth-elektronik/760308101/wireless-power-charging-coil-24uh/dp/2114371) | Example of a commercially orderable litz-wire/ferrite-backed small coil with published L, DCR and Q. | Small receiver coil intended for wireless charging; no hob-level power/temperature qualification or cooking-zone geometry. |
| Würth Elektronik 760308106, Farnell order 2336090 | Three-coil wireless-power array; published inductance 1 × 11.5 µH and 2 × 12.5 µH; listing advertises 10 A; listed €34.30 ex VAT and 50-week manufacturer lead time at survey. [Farnell](https://de.farnell.com/en-DE/wurth-elektronik/760308106/wireless-charging-coil-12-5uh/dp/2336090) | Shows multi-coil commercial assemblies and highlights that listed availability may mean long lead time. | Compact wireless-power array, not a cooking coil; no published hob application or cookware-loaded characterization. |
| Würth Elektronik 760308100111 | WE-WPCC Qi wireless-power coil; Mouser listing reported 401 in stock at time indexed. [Mouser Europe](https://eu.mouser.com/en/ProductDetail/Wurth-Elektronik/760308100111) | Orderable reference for litz-wire wireless-power coil technology. | Small wireless-power product; inventory and Qi/WPT application do not establish induction-cooking suitability. |
| Würth Elektronik 760308101219 | WE-WPCC Qi wireless-power coil; Mouser listing reported 26 in stock at time indexed. [Mouser Europe](https://eu.mouser.com/en/ProductDetail/Wurth-Elektronik/760308101219) | Another distributor-listed WPT coil for comparative research. | Not a hob-sized or cooking-qualified assembly. |
| TDK WR303050-15F5-G | Wireless-charging receiver coil; 12.3 µH; max DCR 0.41 Ω; 30 × 30.4 × 0.92 mm. [Farnell](https://de.farnell.com/en-DE/tdk/wr303050-15f5-g/charging-coil-12-3uh/dp/2377529) | Useful example of an explicitly specified small ferrite-backed coil. | Intended for portable-device wireless charging; no suitable scale or power evidence for a cooking zone. |
| Fair-Rite wireless-charging ferrite plates | Mouser describes plate options including 100 × 100 mm and 150 × 100 mm, 3–8 mm thick, and recommended wireless-power systems from 1.4 kVA to 19 kVA. [Mouser](https://eu.mouser.com/en/new/fair-rite/fair-rite-wireless-charging-ferrite-plates) | Potential lead for investigating commercially available ferrite plate materials and geometries. | These are WPT magnetic plates, not induction-cooktop ferrite bars. Power-system recommendations refer to their WPT context and must not be transferred to cooking applications; material loss, temperature, flux and geometry would need independent assessment. |
| Würth Elektronik WE-HCF 7443631000 | Shielded SMD power inductor; 10 µH; 16 A RMS; 23 A saturation; 21.8 × 21.5 × 14.5 mm. [Farnell](https://de.farnell.com/en-DE/wurth-elektronik/7443631000/induktivit-t-10uh-15-16a-hcf-2013/dp/1869764) | Ordinary power-stage magnetics may be relevant elsewhere in the electronics BOM. | A packaged SMD power inductor is not the cooking work coil and its current rating is not a substitute for high-frequency resonant-coil loss/thermal data. |

## Interpretation

- **No confirmed distributor-sourced hob coil was found in this pass.** This is a bounded search result, not proof that no distributor carries one.
- Small WPT coils can have attractive published Litz-wire, ferrite, DCR and Q data, but their dimensions, winding construction, magnetic circuit, operating frequency and thermal assumptions differ substantially from a hob work coil.
- Ferrite plates for wireless-power transfer are a possible materials-sourcing lead, not drop-in replacements for the ferrite bars and backing arrangement used in domestic induction hobs.
- Standard power inductors belong in a separate electronics BOM, not the cooking-coil dataset.
- Price and stock figures are volatile distributor snapshots. Check the linked product page and datasheet before any purchase.

## Next distributor searches

1. Search Mouser, Farnell, RS, TME and Distrelec for **litz wire by the metre, high-temperature stranded copper, ferrite bars/plates/material grades, temperature sensors, connectors, resonant film capacitors and power semiconductors** as separate component categories.
2. Keep the cooking work coil search focused on OEM induction-hob spare assemblies, manufacturer-supplied evaluation coils, or a custom-wound coil with documented winding and ferrite details.
3. If considering a custom coil, request manufacturer-confirmed data for inductance/impedance versus frequency and cookware, AC resistance/Q, allowable current and temperature, insulation system, sensor and mechanical drawing. Do not infer suitability from nominal inductance or wire current ratings alone.

## Sources

- [Farnell charging coil arrays](https://de.farnell.com/en-DE/c/passive-components/inductors/charging-coil-arrays)
- [Farnell inductors catalogue](https://de.farnell.com/en-DE/c/passive-components/inductors)
- [Mouser Europe ferrite cores and accessories](https://eu.mouser.com/en/c/passive-components/ferrites/ferrite-cores-accessories/)
- [Mouser Fair-Rite wireless-charging ferrite plates](https://eu.mouser.com/en/new/fair-rite/fair-rite-wireless-charging-ferrite-plates)


## Second pass: supporting-component sourcing

This pass broadened the survey from coils to components that could support a custom, documented design. These are **BOM research leads**, not a selected or validated design.

| Component class | Example found | Published details | Relevance and limitation |
|---|---|---|---|
| Induction-heating IGBT | STMicroelectronics STGW30NC120HD, Farnell order 1542228. [Farnell](https://de.farnell.com/en-DE/stmicroelectronics/stgw30nc120hd/igbt-n-1200v-30a-to-247/dp/1542228) | 1200 V device; 60 A continuous collector current shown in listing; manufacturer description explicitly names induction heating and soft switching. | Stronger application relevance than generic IGBTs, but a distributor listing is not a complete switching-loss, SOA, gate-drive, thermal or topology review. Do not treat the headline current as usable resonant current. Confirm lifecycle, current datasheet and availability before any design decision. |
| Polypropylene film capacitor | TDK/EPCOS B32671L9103J000, Farnell order 3519039. [Farnell](https://de.farnell.com/en-DE/epcos/b32671l9103j000/cap-0-01uf-1kv-film-radial/dp/3519039) | 10 nF; metallized polypropylene; 1 kV DC / 250 V AC listing; high-pulse-capability series. | Evidence that pulse-oriented MKP film capacitors are orderable, but the small value and listing alone do not qualify it as a resonant tank capacitor. Current, RMS ripple, dv/dt, ESR, thermal rise and fault mode must be checked against the actual circuit. |
| Polypropylene film capacitor for oscillating circuits | WIMA MKP4-100N/1000, TME. [TME](https://www.tme.eu/en/details/mkp4-100n_1000/tht-film-capacitors/wima/mkp4o131005d00kssd/) | 100 nF; 1 kV DC; 400 V AC maximum stated; TME lists high-frequency and oscillating-circuit applications. | Useful as a datasheet-review lead, not a chosen resonant capacitor. The exact capacitance, AC voltage, ripple-current and pulse-stress ratings may be unsuitable for a hob inverter. |
| Higher pulse-resistance MKP family | WIMA MKP10-47N/1000, TME. [TME](https://www.tme.eu/en/details/mkp10-47n_1000/tht-film-capacitors/wima/mkp1o124704f00kssd/) | 47 nF; 600 V AC / 1 kV DC; listed pulse resistance 2.7 kV/µs. | Useful family to examine for pulse circuits. The pulse-resistance figure alone does not establish repetitive resonant current capability or lifetime. |
| Small NTC sensor | Vishay NTCS0603E3103FMT, TME Germany. [TME](https://www.tme.eu/de/details/ntcs0603e3103fmt/ntc-mess-thermistoren-smd/vishay/) | 10 kΩ, ±1%, B = 3610 K, -40 to 150°C; 0603 package. | Useful for board-level sensing or experimental instrumentation, not automatically suitable for direct attachment to a hot cooking coil or glass stack. Thermal coupling, insulation, response time and open/short fault detection remain design questions. |
| Higher-temperature packaged NTC | TE Connectivity 20029369-01, Farnell. [Farnell](https://de.farnell.com/en-DE/te-connectivity/20029369-01/ntc-sensor-1-196-kohm-at-150deg/dp/4698412) | Cylindrical NTC sensor; -40 to 200°C listed; fluoroplastic protection; intended for motor-system temperature monitoring. | Potentially more relevant mechanically than a bare SMD thermistor, but its application is motor monitoring. Verify the full resistance curve, insulation, response, mounting and suitability for the target environment before considering it. |
| Ferrite core | TDK/EPCOS B66311G0000X127, Farnell order 2355087. [Farnell](https://de.farnell.com/epcos/b66311g0000x127/ferritkern-e-20-n27/dp/2355087) | E20/10/6 geometry, N27 material, effective area 32.1 mm². | Standard small ferrite core useful for auxiliary magnetics or material research, not a substitute for large ferrite bars/backing under a cooking coil. Core loss data must match flux density, frequency and temperature. |
| Ferrite core catalogue | TDK ferrite cores on Mouser Europe. [Mouser](https://eu.mouser.com/en/c/passive-components/ferrites/ferrite-cores-accessories/) | Multiple E, ETD, PM, U, RM and other core families listed. | Useful source for auxiliary transformer/choke designs. The presence of catalogued ferrite does not establish suitability for the work-coil magnetic circuit. |
| NTC temperature probe | Labfacility RAA-S2TH-4.0-150-NP-2.0-C5-T, Farnell. [Farnell](https://de.farnell.com/en-DE/c/w/sensors-transducers/sensors/temperature-sensors-transducers/thermistors/ntc-thermistors?external-diameter=4mm&produktpalette=21853-series) | 10 kΩ at 25°C; B = 3977 K; 4 mm stainless-steel probe; -50 to 150°C listed. | A packaged probe may suit test instrumentation better than an SMD part, but response time and mounting are very different from a hob's embedded coil sensor. The product page and datasheet must be checked for the intended installation. |

### What this changes

1. Add a **supporting-component** sourcing track to the project, separate from the cooking-coil catalogue. Do not inflate the coil dataset with unrelated inductors, small WPT coils, or ferrite cores.
2. For a power-stage shortlist, start from manufacturer application notes/reference designs and then confirm distributor lifecycle/availability. A part being described as "for induction heating" is a promising filter, not proof that it matches PawPlate's intended topology or ratings.
3. For resonant capacitors, shortlist by the manufacturer's repetitive pulse, RMS current, AC voltage, dv/dt, ESR and thermal data—not capacitance and DC voltage alone.
4. Treat the work-coil temperature sensor as part of the mechanical/thermal assembly: temperature range, insulation, response time, fault detection and calibration matter as much as nominal resistance.
5. TME's catalogue contains broad film-capacitor and NTC ranges, and Farnell has dedicated NTC/probe categories. These category pages are useful discovery paths, but individual parts need current datasheet checks.

## Additional distributor links

- [TME polypropylene capacitors](https://www.tme.eu/en/katalog/polypropylene-capacitors_100472/)
- [TME Vishay 10 kΩ NTC thermistor](https://www.tme.eu/de/details/ntcs0603e3103fmt/ntc-mess-thermistoren-smd/vishay/)
- [Farnell NTC thermistors](https://de.farnell.com/en-DE/c/sensors-transducers/sensors/temperature-sensors-transducers/thermistors/ntc-thermistors)
- [Farnell induction-heating IGBT](https://de.farnell.com/en-DE/stmicroelectronics/stgw30nc120hd/igbt-n-1200v-30a-to-247/dp/1542228)


## Third pass: component cross-check against a dual-zone reference design

The Infineon REF-SHA3K3IHWR5SYS user guide is especially useful because it ties named components to a documented two-zone, half-bridge induction-cooking system. The reference design is specified for 230 V input, two zones, 30–50 kHz and roughly 3.3–3.5 kW system capability. That is close enough to PawPlate's architectural study to be a strong *reference*, but its published maximum capability is not evidence that it meets PawPlate's separate 3.0 kW continuous target or that its design can be copied without review. [Reference board](https://www.infineon.com/evaluation-board/REF-SHA3K3IHWR5SYS) · [User guide](https://www.infineon.com/assets/row/public/documents/60/44/infineon-ug-2024-05-smart-induction-cooktop-usermanual-en.pdf)

### Named reference components and sourcing notes

| Reference-design component | Function stated by Infineon | Distributor evidence | Assessment |
|---|---|---|---|
| IHW40N65R6 | Half-bridge induction inverter IGBT pair | Infineon's [official product page](https://www.infineon.com/part/IHW40N65R6) lists the exact reference part as active/preferred and in stock at the time checked. | Better sourcing evidence than the earlier related-variant-only search. Still verify live supply, datasheet revision and distributor pricing before procurement; the part's availability does not validate the power stage. |
| 2ED21824S06J | Half-bridge gate driver | Infineon's [official product page](https://www.infineon.com/part/2ED21824S06J) lists the exact part as active/preferred and describes it as a 650 V, 2.5 A half-bridge driver with integrated bootstrap diode. The page did not show a clear stock quantity in this search snapshot. | Exact part is currently listed by its manufacturer; stock and lifecycle evidence should be rechecked at procurement. Confirm the full driver design and gate network together. |
| TLI4971 family (guide copies disagree) | Current sensing for pan detection, coil-current measurement and overcurrent detection | Publicly indexed copies of the guide list A120T5-U-E0001 and A120T5-U-F0001, while another section names an A075 variant. Infineon's [TLI4971-A120T5-U-E0001 product page](https://www.infineon.com/part/TLI4971-A120T5-U-E0001) lists the 120 A part as active/preferred and in stock, with typical 240 kHz bandwidth, diagnosis features, dual fast overcurrent outputs, and planned availability until at least 2038. Its datasheet identifies ordering code SP005272936. | The A120T5-U-E0001 is a currently offered sourcing lead, but exact reference-board fit is not fully established from conflicting guide copies. Confirm the schematic/BOM revision before any shortlist or purchase. A sensor's fast overcurrent outputs are not, by themselves, an independent appliance safety shutdown. |
| CY8C6244AZI-S4D82 | MCU on the inverter control board | Infineon's [official product page](https://www.infineon.com/part/CY8C6244AZI-S4D82) lists the exact MCU as active/preferred. A [Mouser listing](https://www.mouser.ie/en/ProductDetail/Infineon-Technologies/CY8C6244AZI-S4D82) previously showed it as non-stocked with an estimated lead time. | Manufacturer lifecycle status is positive, but distributor availability may be less convenient than the ESP32 family. This is a reference-controller option, not a reason to change PawPlate's ESP32-family decision. |
| ICE5QR1680BG | Auxiliary flyback supply controller (two listed in the reference guide) | Infineon's [official product page](https://www.infineon.com/part/ICE5QR1680BG) lists the exact part as active/preferred and in stock; it integrates an 800 V MOSFET in a flyback controller. | Useful sourcing evidence for the auxiliary-supply block only. It does not supply a complete isolated multi-rail design; transformer, isolation, creepage, thermal and fault behaviour remain design-specific. |

### Important interpretation

- This reference guide is a more credible starting point than selecting unrelated distributor parts independently: it identifies a real two-zone half-bridge architecture and names key sensing, switching and control parts.
- It still does **not** make a complete, validated PawPlate BOM. Coil, resonant capacitor, bus capacitor, EMI filter, gate resistors, thermal stack, sensors, connectors, layout and protection must all be evaluated as a coordinated system.
- The Infineon page advertises 3.3 kW output in its product data and 3.5 kW support in its features. PawPlate's 3.0 kW target is appliance input power, so ratings must be compared only after defining whether each figure is input or delivered heating power.
- Public PDFs are available, while Infineon notes that registered users can access additional BOM/design files. A community answer says PCB layout and schematic PDFs are publicly accessible, but editable CAD/Gerber availability was not confirmed. [Infineon developer community discussion](https://community.infineon.com/t5/IGBT/Looking-for-PCB-Design-Files-of-Smart-Induction-Cooktop-Reference-Design-REF/td-p/794882)
- The user guide describes the MCU and current sensor roles; any safety-critical overcurrent trip and gate-disable path still needs independent analysis and cannot depend solely on ordinary firmware execution.

## Recommended architecture research direction

Use REF-SHA3K3IHWR5SYS as the leading **system-level reference** for a dual-zone half-bridge, and use ST AN4713 and Infineon's single-zone evaluation boards as independent topology/characterization references. Keep the ESP32 in the candidate architecture until the required PWM timing, synchronized ADC sampling, comparator/trip resources, watchdog behavior and fail-safe outputs are compared against actual ESP32 variants. Do not choose an MCU based on connectivity features.

Next useful work is a source-backed functional-block comparison rather than a premature BOM: (1) power switch + gate driver, (2) coil/tank + sensing, (3) current/voltage/temperature protection, (4) auxiliary supplies and mains input protection, (5) controller and independent hardware disable, and (6) dual-zone input-power budget. For each block, track exact reference part, manufacturer documentation, distributor availability, and what remains unverified.
