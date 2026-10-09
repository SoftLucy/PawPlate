# Induction coil assembly availability and electrical comparison

Date: 2026-10-09  
Status: desk research only. This is a candidate survey, not an exhaustive global inventory and not an approval list.

## Executive finding

Commercial replacement assemblies are easy to find by hob model and often list diameter and nominal zone power, but public listings rarely publish the electrical parameters needed to qualify a coil for a different inverter. The strongest public numerical dataset found in this survey is a research paper describing a constructed 180 mm coil. The strongest practical candidate for a documented reference is the replaceable 170 mm coil supplied with Infineon's EVAL-IHW25N140R5L evaluation board, but its public product page does not expose a complete coil parameter sheet; Infineon says the supplied coil is sized for that board's resonant tank.

**Do not infer compatibility from diameter or wattage.** The coil, ferrite, cookware, resonant capacitor network, inverter topology and control limits form one coupled system. Any candidate below remains unapproved until the actual assembly is characterized and tested with a specific power stage.

## Candidates found

| Candidate | Publicly stated data | Electrical data missing | Assessment for PawPlate |
|---|---|---|---|
| Infineon EVAL-IHW25N140R5L supplied 170 mm cooking coil | 170 mm; replaceable; bundled with a 2 kW single-ended/quasi-resonant induction-heating evaluation board. Vendor says coil has an inductance value sized for its resonant tank. [Product page](https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L), [user-manual scope of supply](https://www.manualslib.com/manual/3364510/Infineon-Eval-Ihw25n140r5l.html?page=6) | Public page/manual extract found in this survey does not state numeric L, AC resistance/Q versus frequency, winding construction, current/temperature limits, sensor part/curve or complete mechanical drawing. | **Best reference candidate for later characterization**, because it is a documented replaceable assembly tied to a known inverter. Not automatically compatible with a half-bridge design. |
| Midea 17466000000118 replacement coil | 180 mm diameter; listed as 2000 W; marketed as a spare coil for various hob brands/models. [FixPart listing](https://fixpart.de/produkt/midea-17466000000118-induktionsplatte) | Inductance, AC resistance, winding geometry, ferrite specification, sensor details/curve, rated frequency/current, thermal limits and full drawing not published on the listing. | **Commercially available replacement candidate, low electrical transparency.** Buy only after confirming exact part identity, mechanical package and whether manufacturer/service data can be obtained. |
| Midea-derived replacement coil 04Q46894498 | 180 mm diameter; 2000 W; listing associates it with PKM IF4N 23104. [Ersatzteilcheck24](https://www.ersatzteilcheck24.de/geraete/-I-3583134.html) | Same critical electrical data absent from public listing. It may be a related/overlapping spare family, but equivalence to Midea 17466000000118 is not established by this survey. | **Do not count as a separate electrically characterized design** until the part numbers and construction are confirmed. Useful evidence that 180 mm / 2 kW spare assemblies exist, not evidence of interchangeability. |
| Electrolux 'Domino with Tiger' service-manual assemblies | The service manual documents separate 140 mm and 180 mm magnetic induction coil assemblies. It shows NTC sensing, coil leads, insulation, support and spring mounting, and describes coil removal. [Service manual](https://www.manualslib.com/manual/1778696/Electrolux-Domino-With-Tiger.html) | The public manual text found here does not provide coil inductance, AC resistance, turns/wire data, rated coil current or a complete standalone coil part-number cross-reference. The coils are part of a particular generator/hob system. | **Useful repairability/mechanical reference**, not an electrically qualified third-party coil. Obtain exact appliance and spare-part numbers before further sourcing work. |
| Published 180 mm research coil (not a confirmed purchasable assembly) | Research paper gives 28 turns; 180 mm outside diameter; 30 mm inner diameter; coil-to-ferrite spacing 4 mm; coil-to-pan spacing 4 mm; 66 strands of 0.27 mm litz wire; ferrite relative permeability 800. No-pan equivalent L = 110 µH and R = 0.12 Ω. With cast-iron pan: L = 89.76 µH and R = 4.21 Ω. With stainless-steel pan: L = 81.81 µH and R = 3.36 Ω. [Paper: Electronics 12(19), 4145](https://www.mdpi.com/2079-9292/12/19/4145) | These are equivalent parameters for the paper's specific test/model conditions, not a vendor datasheet or universal coil rating. Current, thermal rating, sensor, manufacturing tolerance and production availability are not established. | **Best numerical design reference found**, but treat as a research reference only, not a sourced replacement part. |
| Custom industrial induction coils | Custom assemblies are available from specialist system manufacturers; examples are application-specific rather than standardized domestic hob replacements. [ITG Induktion](https://www.itg-induktion.de/en/induction-systems/assemblies-for-induction-systems) | Values depend on the customer's application; no common default L/R/current/frequency rating can be assumed. | Potential future custom route, likely poor fit for a zero-budget first iteration unless a supplier supports a research project. |

## Electrical comparison: what the numbers mean

The only candidate in this survey with a useful set of published coil electrical values is the research coil. The commercial assemblies mostly publish only diameter and appliance power class.

| Parameter | Research 180 mm coil | Commercial 180 mm Midea listings | Infineon supplied 170 mm coil |
|---|---:|---|---|
| Nominal outside diameter | 180 mm | 180 mm | 170 mm |
| Advertised appliance/board power | Research setup; do not infer a product rating | 2000 W listing | Board supports up to 2 kW; this is not a separate coil power rating |
| Turns / conductor | 28 turns; 66 × 0.27 mm litz strands | Not published | Not published in the public page/manual extract reviewed |
| Inductance, no pan | 110 µH | Not published | Numeric value not published in reviewed source |
| Inductance, cookware present | 89.76 µH cast iron; 81.81 µH stainless steel | Not published | Not published |
| Equivalent resistance, no pan | 0.12 Ω | Not published | Not published |
| Equivalent resistance, cookware present | 4.21 Ω cast iron; 3.36 Ω stainless steel | Not published | Not published |
| Ferrite geometry/material | Spacing 4 mm; reported relative permeability 800 | Not published | Not published in reviewed source |
| Sensor specification | Not reported in the cited parameter table | Not published | Not established from the public product listing |
| Explicit approved inverter | Research paper's own test/model setup | Specific OEM hob model(s) only | EVAL-IHW25N140R5L board and its original resonant tank |

**Important interpretation:** the paper's 'equivalent resistance' with a pan is not simply the DC resistance of the copper winding. It represents the loaded coil/cookware system under the paper's model and test conditions. These values cannot be compared as if they were measured with the same method as a multimeter's winding-resistance reading. Inductance also changes with cookware and measurement conditions.

## Why nominal wattage is insufficient

Two 180 mm coils both advertised for 2000 W may differ in turns, litz construction, ferrite shape, winding-to-pan distance, parasitic capacitance, sensor placement and inductance under cookware. These affect resonant frequency, current, losses, pan detection, heating distribution and switch stress. The power figure is normally a zone/application rating, not a complete coil electrical rating.

## What data to request from a supplier

Before a candidate can enter the PawPlate compatibility process, request:

1. Exact part number, revision, manufacturer and traceable datasheet/drawing.
2. Coil dimensions, mounting envelope, ferrite material and geometry.
3. Inductance measured with specified instrument/frequency and both unloaded and defined cookware conditions.
4. Winding DC resistance at a stated temperature **and**, ideally, AC resistance or Q versus frequency across the intended operating band.
5. Conductor/litz construction, rated current, frequency range and permitted coil temperature.
6. Integrated temperature sensor part number, resistance curve/tolerance and placement.
7. Insulation, lead/connector and thermal ratings, including the intended mounting stack-up.
8. The OEM inverter/resonant capacitor combination and any published compatibility or service data.

If a supplier cannot provide these data, mark the assembly **mechanically described / electrically uncharacterized**, not compatible.

## Recommended next step

Use the Infineon 170 mm supplied coil as the first reference assembly to investigate because it comes with a known evaluation platform and is explicitly replaceable. Separately retain the published 180 mm research coil as a numerical benchmark. Do not choose a production coil yet. First settle the candidate inverter topology and operating band, then obtain an exact coil sample and characterize it under a documented, qualified test plan. Do not run experiments on mains-powered resonant hardware without suitable expertise and test facilities.

## Sources and limitations

- Infineon, [EVAL-IHW25N140R5L product page](https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L): 2 kW single-ended board, 170 mm replaceable coil.
- Infineon EVAL-IHW25N140R5L manual, [scope of supply](https://www.manualslib.com/manual/3364510/Infineon-Eval-Ihw25n140r5l.html?page=6): supplied coil is sized for the resonant tank; different coils may be tested on the evaluation platform, but that is not a blanket safety/compatibility approval.
- FixPart, [Midea 17466000000118](https://fixpart.de/produkt/midea-17466000000118-induktionsplatte): 180 mm, 2000 W listing.
- Ersatzteilcheck24, [PKM IF4N 23104 replacement coil](https://www.ersatzteilcheck24.de/geraete/-I-3583134.html): 180 mm, 2000 W listing.
- Electrolux, [Domino with Tiger service manual](https://www.manualslib.com/manual/1778696/Electrolux-Domino-With-Tiger.html): 140 mm and 180 mm assemblies, NTC and mounting/service details.
- 'A Simplified Design Method for Quasi-Resonant Inverter Used in Induction Hob', *Electronics* 12(19), 4145 (2023), [DOI / article](https://www.mdpi.com/2079-9292/12/19/4145): numerical coil parameters and cookware-dependent equivalent values.
- ITG Induktion, [induction system assemblies](https://www.itg-induktion.de/en/induction-systems/assemblies-for-induction-systems): example of application-specific custom assemblies.

This is a targeted web survey, not proof that these are all products available worldwide. Listings and spare-part availability can change; electrical specifications not present in the cited sources are explicitly left unknown rather than guessed.
