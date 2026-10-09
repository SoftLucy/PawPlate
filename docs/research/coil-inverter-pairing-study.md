# Coil-first inverter selection study

Date: 2026-10-09
Status: research decision support; no hardware has been built or validated. Prices/stock are snapshots from supplier pages and may change.

## Executive decision

PawPlate should select a **coil/inverter/resonant-network combination as one candidate system**, with coil availability and replaceability used as early filters. The research does not justify selecting a final coil or inverter yet.

Keep three different roles distinct:

1. **Two-zone architecture reference:** Infineon REF-SHA3K3IHWR5SYS, a dual-hob half-bridge reference design rated in the product page at 3.3 kW and 30-50 kHz. Its public design files are PDF-oriented and the kit is comparatively expensive and supply-constrained.
2. **Independent dual-zone technical reference:** Texas Instruments SPRABT2, a detailed application report describing two half-bridge resonant channels, variable-frequency control and detection/protection concepts. It is old and not a complete open, reproducible design package.
3. **Matched coil / measured-load reference:** Infineon EVAL-IHW25N140R5L includes a replaceable 170 mm coil and sensor assembly with a 2 kW, 220 VAC single-zone quasi-resonant inverter. It has useful published load curves and is relatively low-cost and sourceable. It is a characterization/reference kit, not PawPlate's chosen topology.
Also retain Infineon's EVAL_2KW_SiC_IH as a numerical high-frequency coil reference, and Midea 17466000000118 as a cheap, currently listed 180 mm / 2 kW replacement candidate whose electrical data still needs to be obtained or measured.

**Recommendation:** do not buy a power board or select a coil permanently yet. Use the available reference designs to set up a coil qualification path: seek complete data for the exact Midea spare and the Infineon supplied 170 mm coil; keep the Infineon dual-hob and TI dual-half-bridge designs as architecture references. After the candidate coil and topology are selected, define measured acceptance limits and the appliance power budget.

## Why choose the coil and inverter together?

The coil is one element in a coupled resonant system. Its effective impedance changes with frequency, cookware material/diameter, distance, ferrite construction and mechanical stack-up. Those changes affect resonant frequency, output power, current, losses and switch commutation. Infineon's evaluation manuals explicitly warn that changing the coil or vessel changes the load properties and requires individual measurement. The 2023 half-bridge series-resonant research study similarly recommends measuring coil impedance both with and without cookware across possible operating frequencies before selecting the resonant network.

The selection unit is therefore:

**coil cassette + ferrite + cookware gap + resonant capacitor network + inverter topology/devices + control and sensing + thermal/mechanical stack-up.**

A coil's advertised wattage is not a universal power rating when transplanted to another inverter. A coil that fits a zone mechanically is not automatically a valid electrical replacement.

## Candidate systems and paths

### A. Infineon REF-SHA3K3IHWR5SYS - strongest direct two-zone architecture reference

Official product page:
https://www.infineon.com/evaluation-board/REF-SHA3K3IHWR5SYS

Product-page details found:
- Described as a dual-hob half-bridge Smart Induction Cooktop reference design.
- Published frequency range: 30-50 kHz.
- Product page lists 3.3 kW Pout, 220 V and 15 A, and separately advertises support for 3.5 kW. The user guide's own technical table states maximum 3,600 W **system input** at 220 V AC; it lists the bigger coil at 2,200 W (3,000 W boost), the smaller coil at 1,400 W (2,000 W boost), and coil inductances of 51 µH and 71 µH. These labels and conditions are not interchangeable; use the manual's explicit input-power statement when comparing against PawPlate's input target.
- Product family includes PSoC 6; the user guide is public.
- Inverter schematic and PCB layout PDFs exist. Infineon staff stated in the community that publicly downloadable board files were PDF-only, not editable CAD/Gerber. However, Infineon's [ModusToolbox Smart Induction Cooktop Pack](https://softwaretools.infineon.com/tools/com.ifx.tb.tool.modustoolboxpacksmartinductioncooktop) advertises hardware Gerber files, firmware and board support packages behind login/request gating. Editable files may therefore be obtainable through an access request, but access and reuse rights are not confirmed. Some BOM/design files are also gated.
- A Mouser listing was indexed at EUR 804.96, with no stock and units on order / long lead time at the time of the result. Verify live availability and the precise kit contents before relying on these figures.

Why it matters:
- It is the closest identified current vendor reference to a dual-zone, half-bridge architecture near PawPlate's target size.
- The design gives a concrete source for power electronics, gate driving, sensing and HMI integration decisions.
- It reduces the risk of making architecture choices without an existing dual-hob comparator.

Limitations:
- No complete numeric coil impedance dataset or independently reorderable coil part number was established from the public material reviewed.
- Its controller is PSoC 6, not ESP32.
- Publicly available PDFs are useful for learning but less useful for direct repairable/open-source development than native editable sources and a clear reuse license.
- The manual's 3,600 W maximum system-input rating at 220 V AC is the most directly comparable published figure, but it is above PawPlate's 3.0 kW continuous input target and must not be assumed compatible with a 230 V / 16 A installation. Product-page 3.3 kW Pout and 3.5 kW support figures have different labels/conditions. PawPlate needs its own measured/verified input budget and zone-sharing limits.

Primary sources:
- https://www.infineon.com/evaluation-board/REF-SHA3K3IHWR5SYS
- https://www.infineon.com/assets/row/public/documents/60/44/infineon-ug-2024-05-smart-induction-cooktop-usermanual-en.pdf
- https://community.infineon.com/t5/IGBT/REF-SHA3K3IHWR5SYS/td-p/793733
- https://www.infineon.com/gated/infineon-schematics-ref-sha3k3ihwr5sys-inverter-board-pcbdesigndata-en_736d2c96-b626-4eab-b929-b0211b226939

### B. Texas Instruments SPRABT2 - useful independent two-zone control reference

Application report:
https://www.ti.com/lit/an/sprabt2/sprabt2.pdf

The 2013 report describes one C2000 MCU controlling two resonant half-bridge converters. It documents:
- 220 VAC / 50 Hz input context.
- Separate resonant channels and fans for the two zones.
- Variable switching frequency from 30 to 70 kHz.
- Approximate 50% complementary PWM with dead time.
- Current-transformer feedback to regulate current/power.
- NTC feedback for IGBT and plate temperatures.
- Pot detection from the current transient / resonant response.
- ZVS checks, overcurrent and overtemperature response, and a safety relay in the input path.
- An approximate 150 mm x 175 mm board-space target for a dual-zone version.

Why it matters:
- The document is unusually useful for understanding a dual-channel control architecture, not just a single inverter.
- Its 30-70 kHz range overlaps the 30-50 kHz published for the Infineon dual-hob design and ST's 22-34 kHz test example, although overlap in frequency is not proof of coil compatibility.
- It is a good independent check that a two-half-bridge architecture and one real-time controller are plausible.

Limitations:
- It dates from 2013, uses an older C2000 controller, and is an application report rather than a complete purchasable matched board/coil kit.
- The material reviewed does not establish an exact coil part number with published L/R/Q curves, nor a contemporary, reproducible native CAD+BOM+source package.
- Its reported control/protection techniques are not a safety certification. PawPlate should retain independent hardware fault shutdown; any MCU-mediated protection in a reference design should not be treated as sufficient for our architecture.

### C. Infineon EVAL-IHW25N140R5L - most practical 20-50 kHz matched-coil reference kit

Official page and user manual:
- https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L
- https://www.infineon.com/dgdl/Infineon-EVAL-IHW25N140R5L-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8afe5bd0018b18c78e023a8d

Published details:
- Up to 2 kW output at 220 VAC, single-zone quasi-resonant topology.
- 170 mm replaceable cooking coil supplied with a temperature sensor as part of the coil assembly.
- The coil is sized for the board's resonant tank.
- The manual includes load-impedance curves across 0-50 kHz with and without cookware.
- Protections listed include overvoltage, undervoltage, overcurrent and overtemperature.
- The manual states complete schematic/BOM downloads are on the support page and require login.
- Supplier results indexed prices around EUR 103 ex VAT (Farnell) and EUR 108.13 (Mouser Germany), with stock shown in those search snapshots. Stock/prices must be verified at purchase time.

Why it matters:
- This is the lowest-friction way identified to obtain a real coil, sensor assembly and corresponding operating reference as a matched set.
- It gives PawPlate an actual load curve in the frequency band used by many domestic induction cooking designs.
- A complete kit makes the coil easier to inspect and compare than a bare OEM replacement listing.

Limitations:
- It is single-zone, quasi-resonant and limited to 2 kW output. It is not the preferred final two-zone half-bridge architecture.
- Its manual says the board is for evaluation by skilled technical staff, not a safety-qualified production appliance. It says its PCB and auxiliary circuits are not optimized for final customer design.
- The manual recommends a 5-10 mm coil-to-vessel distance to avoid excessive hard-switching events on its setup; do not import that geometry into PawPlate blindly. The final glass/coil spacing must be established with our chosen design.
- It cannot prove the supplied coil will work with a different resonant network or half-bridge channel.

Decision role: keep this as **the first matched-coil characterization reference**, not the final inverter selection.

### D. Infineon EVAL_2KW_SiC_IH - best published high-frequency coil curves, weaker domestic fit

Official page and application note:
- https://www.infineon.com/evaluation-board/EVAL-2KW-SIC-IH
- https://www.infineon.com/assets/row/public/documents/24/42/infineon-eval-2kw-sic-ih-applicationnotes-en.pdf

Published details:
- 2 kW half-bridge evaluation board using 650 V SiC MOSFETs.
- 100-140 kHz operating range in the manual; product summary states up to 150 kHz.
- Supplied resonant coil and cookware-based characterization.
- The public graph gives approximate equivalent series parameters: about 49 uH and 0.2-0.35 ohm without the pan; about 18-20 uH and 3.6-4.9 ohm with a pan over approximately 90-150 kHz. These are approximate readings from a plotted graph, not guaranteed specification limits.
- The power board accepts an external 0-340 V DC main supply and an 18 V auxiliary supply. The kit does not include a main-control MCU; the app note describes using an XMC1300 BootKit and firmware.
- Detailed Altium design files are available through the myInfineon login flow.
- Mouser Germany indexed a price of EUR 321.98, zero stock and on-order supply / long lead time in the search snapshot.

Why it matters:
- It demonstrates what a better coil data package looks like: a supplied assembly, measured impedance curves and stated load conditions.
- The modular driver daughter cards and published performance data are relevant to PawPlate's repairability goals.

Limitations:
- It is not a direct 230 V AC hob front end: external DC supply, control and appliance safety/EMC work remain.
- The operating frequency is substantially higher than the 30-50 kHz dual-hob candidate and the EVAL-IHW coil reference. A coil cannot be transferred between them on the basis of diameter or power rating.

Decision role: **reference source for measurement methodology and resonant-load characterization**, not the first candidate for a domestic two-zone architecture.

### E. Midea 17466000000118 - cheapest readily listed 180 mm replacement candidate

Listing:
https://fixpart.de/produkt/midea-17466000000118-induktionsplatte

The listing describes an original 180 mm, 2,000 W induction coil and listed it at EUR 19.44 including VAT (shipping extra), in stock with a 1-2 business day estimate when searched. The spare part is associated with many hob/appliance brands and models.

Why it matters:
- Low price, an identifiable part number and an established spare-parts listing are attractive for repairability.
- A 180 mm assembly is a plausible size candidate for a compact domestic zone.

What is not known from the listing:
- Inductance/impedance across frequency, with and without cookware.
- DC and AC winding resistance/Q, wire/litz construction and current rating.
- Ferrite construction and geometry.
- Sensor exact part/curve/location, coil temperature limits, lead/connector ratings and detailed mechanical drawing.
- Exact validated inverter/resonant-network combination beyond its OEM appliance use.

Decision role: keep as a **low-cost commercial coil candidate for supplier-data requests or later characterization**. Do not call it compatible with the PawPlate inverter from the listing alone.

### F. ST AN4713 and published academic data - useful electrical baselines, not products

ST application note AN4713:
https://www.st.com/resource/en/application_note/an4713-induction-cooking--igbts-in-resonant-converters-stmicroelectronics.pdf

The note reports a half-bridge series-resonant converter tested from 1 to 3 kW with generic induction cookware. The documented example uses a resonant inductance of 80 uH measured at 10 kHz with no load, a 680 nF resonant capacitor, a 33 nF snubber and switching frequency of 22-34 kHz at up to 3 kW. It reports substantial current and thermal stress and assumes strong cooling/test conditions. It is a component/topology example, not a coil supplier specification.

Open-access research:
https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/pel2.12503

The 2023 half-bridge study measures a different coil across 15-50 kHz. Its unloaded inductance is about 79.8 uH; with a pan, reported equivalent inductance declines from 63.2 uH at 15 kHz to 49.3 uH at 50 kHz, with equivalent resistance rising from 1.57 ohm to 3.78 ohm. These conditions and values differ from the Infineon evaluation coils. They provide a useful reminder that the numerical parameters are coil- and cookware-specific, not evidence that the assemblies are interchangeable.

## Side-by-side comparison

| Candidate | Intended role | Topology / frequency | Coil included / useful electrical data | Two-zone fit | Main blocker |
|---|---|---|---|---|---|
| Infineon REF-SHA3K3IHWR5SYS | Architecture reference | Half-bridge; 30-50 kHz | Exact numeric coil curve/part not confirmed in public data reviewed | High | Higher kit cost; PDF/gated design material; coil details unclear |
| TI SPRABT2 | Architecture/control reference | Two half-bridges; 30-70 kHz | Detailed control discussion; no exact sourced coil parameters found | High | Old report; not a complete current sourceable system |
| Infineon EVAL-IHW25N140R5L | Matched coil characterization | Single-switch QR; 0-50 kHz load graph | Yes; 170 mm + sensor, load curves | Low for final appliance, high for learning | Single zone / different topology / evaluation-only |
| Infineon EVAL_2KW_SiC_IH | Electrical/measurement reference | Half-bridge SiC; 100-140 kHz | Yes; approximate L and R curves with/without pan | Low to medium | External DC source, no MCU, higher frequency/cost |
| Midea 17466000000118 | Serviceable low-cost coil candidate | OEM topology unknown | 180 mm / 2 kW marketed; no critical electrical curves | Unknown until characterized | Electrical construction/data absent |
| ST AN4713 / IET research | Design equations and expected envelopes | Half-bridge; roughly 22-50 kHz examples | Electrical measurements on specific generic/research setups | Medium as documentation | Not an obtainable production coil/system |

## Project-specific constraints and implications

### Power budget

PawPlate's requirement is 3.0 kW continuous **total appliance input** and a provisional 3.68 kW peak input ceiling (230 V x 16 A), subject to installation suitability. Do not compare that directly with an evaluation board's resonant output/coil power. The target must include losses, both zones, fan and auxiliary supply loads. A dual-hob reference with a higher advertised output rating can inform control architecture but must be power-limited and redesigned/validated for PawPlate's actual input budget.

### ESP32 role

The references use controller architectures tailored to resonant power control (PSoC 6, C2000 or XMC1300). PawPlate's ESP32 decision should be revisited at the control partition stage. An ESP32 may suit supervisory functions, UI, logging and power scheduling, but it must not be assumed suitable as the sole high-speed resonant controller or sole protection path without analysis of PWM/ADC timing, deterministic fault response, hardware trip mechanisms and safe-default behaviour. A dedicated real-time controller or independent hardware fault circuitry may be warranted.

### Repairability and open source

The easiest board to buy is not necessarily the best design to reproduce. Favor exact part-number sourcing, replaceable coil/sensor cassettes, standardized zone interfaces and published electrical profiles. Vendor evaluation designs are references, not automatically open-source hardware; PDF files and public product pages do not grant rights to reuse proprietary layouts. PawPlate should develop its own documented interface and design or explicitly confirm applicable reuse terms before copying any design files.

## Recommended course of action

1. **Keep half-bridge series-resonant as the leading topology candidate**, not a finalized decision. The dual-hob Infineon board and TI dual-half-bridge report give better two-zone precedent than adapting a single-switch quasi-resonant board as the final product.
2. **Keep two coil paths open:**
   - A 170 mm coil supplied with EVAL-IHW25N140R5L as the practical 20-50 kHz characterization reference.
   - Midea 17466000000118 (180 mm) as a cheap, obtainable replacement candidate whose electrical information needs to be requested or measured.
   Do not yet declare either coil compatible with the half-bridge candidate.
3. **Request data before spending money:** ask Midea/FixPart and, if useful, Infineon for exact drawings, sensor data, impedance versus frequency/cookware, winding construction and temperature/current limits for exact coil part numbers.
4. **Compare resulting measured data against the intended inverter operating envelope.** Only then establish capacitor-network limits, power/current limits and coil compatibility profiles.
5. **Do not buy the EUR 805 dual-hob kit just to get started.** It is expensive and its public documentation is mainly PDFs. First use the free dual-zone reports and public coil data to reduce uncertainties.
6. **No mains experimentation is proposed.** Any subsequent physical characterization of powered resonant circuits requires qualified engineering, suitable laboratory instruments, safe test facilities and independent protection; this report is not a construction design.

## Sources reviewed

- Infineon REF-SHA3K3IHWR5SYS: https://www.infineon.com/evaluation-board/REF-SHA3K3IHWR5SYS
- Infineon REF dual-hob user manual: https://www.infineon.com/assets/row/public/documents/60/44/infineon-ug-2024-05-smart-induction-cooktop-usermanual-en.pdf
- Infineon user discussion on publicly supplied PDF design files: https://community.infineon.com/t5/IGBT/REF-SHA3K3IHWR5SYS/td-p/793733
- TI C2000 Dual VF Resonant Induction Cookers, SPRABT2: https://www.ti.com/lit/an/sprabt2/sprabt2.pdf
- Infineon EVAL-IHW25N140R5L product/manual: https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L and https://www.infineon.com/dgdl/Infineon-EVAL-IHW25N140R5L-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8afe5bd0018b18c78e023a8d
- Infineon EVAL_2KW_SiC_IH product/application note: https://www.infineon.com/evaluation-board/EVAL-2KW-SIC-IH and https://www.infineon.com/assets/row/public/documents/24/42/infineon-eval-2kw-sic-ih-applicationnotes-en.pdf
- Midea 17466000000118 listing: https://fixpart.de/produkt/midea-17466000000118-induktionsplatte
- ST AN4713: https://www.st.com/resource/en/application_note/an4713-induction-cooking--igbts-in-resonant-converters-stmicroelectronics.pdf
- Hsieh (2023), half-bridge series-resonant induction cooker: https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/pel2.12503

This is a focused survey of prominent public references and purchasable examples, not proof that every vendor/coil assembly has been found. Supplier stock and prices are volatile; unreported specifications are left unknown rather than estimated.
