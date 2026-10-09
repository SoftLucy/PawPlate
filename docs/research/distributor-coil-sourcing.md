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
