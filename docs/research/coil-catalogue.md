# Coil assembly catalogue

**Dataset:** [`data/coil-assemblies.csv`](../../data/coil-assemblies.csv)
**Initial survey date:** 2026-10-09
**Status:** early research catalogue; not exhaustive; not an approval list.

## Purpose

Collect findable induction-cooking coil assemblies and relevant numerical research references in one structured place, so PawPlate can choose the inverter and coil as a paired system based on sourcing, documentation, electrical characteristics, repairability and compatibility evidence.

This is a discovery dataset, not a universal interchangeability table. A part listed as 180 mm / 2 kW is not necessarily electrically similar to another 180 mm / 2 kW part.
The initial CSV contains 19 records across:
## Current initial coverage

The initial CSV contains 17 records across:
- matched evaluation-board coil assemblies with manufacturer documentation;
- commercial OEM replacement assemblies with listed part numbers and dimensions/power where published;
- service-manual part references;
- one research coil with numerical inductance/resistance data;
- one conventional radiant hotplate deliberately marked as an exclusion example to prevent search-result confusion.

The initial catalogue intentionally includes records with sparse electrical data. Their missing fields are a research to-do list, not values to infer.

## Dataset schema

| Field | Meaning |
|---|---|
| `record_id` | Stable internal identifier for this record. |
| `record_type` | Matched evaluation assembly, OEM spare, service-manual reference, research coil, or exclusion/verification record. |
| `manufacturer`, `part_number`, `assembly_or_model` | Exact identity as published; do not merge similar-looking part numbers without evidence. |
| `diameter_mm`, `dimensions_or_geometry` | Only dimensions explicitly stated by the source. Keep unusual manufacturer descriptors verbatim if their meaning is unclear. |
| `nominal_power_w`, `rated_voltage_v` | Published application/assembly values, not an assumed standalone coil rating. |
| `inductance_no_pan_uH`, `inductance_with_pan_uH` | Values only when a source publishes them. Cookware type and conditions belong in notes. |
| `equivalent_resistance_no_pan_ohm`, `equivalent_resistance_with_pan_ohm` | Published equivalent-system values. Do not treat pan-loaded equivalent resistance as winding DC resistance. |
| `measurement_frequency_hz`, `operating_frequency_hz` | Measurement or operating band when known; prose ranges are retained where one numeric field would misrepresent the source. |
| `turns`, `conductor_or_litz`, `ferrite_data` | Winding and magnetic details only when documented. |
| `temperature_sensor` | Sensor presence/type only when source-backed; exact part and curve still need to be captured where available. |
| `price_eur`, `availability_snapshot` | Source snapshot only, not a guarantee; recheck before procurement. |
| `electrical_data_completeness` | Qualitative assessment of published electrical data, not safety approval. |
| `source_url`, `notes` | Provenance and caveats. |

## Initial sources and records

### Matched evaluation assemblies

- Infineon EVAL-IHW25N140R5L: replaceable 170 mm coil, 2 kW single-ended evaluation board; load curves in its manual. [Manufacturer](https://www.infineon.com/evaluation-board/EVAL-IHW25N140R5L)
- Infineon EVAL_2KW_SiC_IH: supplied coil with published approximate load curves; high-frequency half-bridge evaluation board. [Manufacturer](https://www.infineon.com/evaluation-board/EVAL-2KW-SIC-IH)
- Published research coil: 180 mm outer diameter, 28 turns, 66-strand 0.27 mm litz wire, no-pan inductance 110 µH, and cookware-dependent values. [Paper](https://www.mdpi.com/2079-9292/12/19/4145)

### Commercial OEM replacement assemblies

- Midea 17466000000118: 180 mm / 2000 W listing. [FixPart](https://fixpart.de/produkt/midea-17466000000118-induktionsplatte)
- Youlong Z303060018: 180 mm induction coil. [FixPart](https://fixpart.de/produkt/youlong-z303060018-induktionsplatte)
- Hisense/Gorenje 898605, GMI1.0 D230-L120: 210 mm. [FixPart](https://fixpart.de/produkt/hisense-gorenje-898605-induktionsplatte)
- Bravo Electric 202020001: 210 mm induction-heating assembly. [FixPart](https://fixpart.de/produkt/bravo-electric-202020001-induktionsplatte)
- Amica 8518032: 160 mm, with manufacturer descriptor preserved verbatim. [FixPart](https://fixpart.de/produkt/amica-8518032-induktionsplatte)
- Hisense/Gorenje 705615: 160 mm / 2100 W listing. [FixPart](https://fixpart.de/produkt/hisense-gorenje-705615-induktionsplatte)
- Beko/Grundig/Arcelik 162000022: 145 mm. [FixPart](https://fixpart.de/produkt/beko-grundig-arcelik-162000022-induktionsplatte)
- Beko/Grundig/Arcelik 162000261, Elica PST0122934 and Electrolux/AEG 3877612444: listed coil parts whose complete electrical and dimensional data still need research.
- Whirlpool/Indesit 481010549107 / C00312205: retailer lists a 145-210 mm range; construction and exact geometry need confirmation. [FixPart](https://fixpart.de/produkt/whirlpool-indesit-481010549107-induktionsplatte)

### Service-manual references

- LEX EVI 640F: separate left/right 180 x 180 mm coil assemblies with thermistors, part numbers 1060077 and 1060089. [Service manual](https://www.manualslib.com/manual/1633781/Lex-Evi-640f.html)
- Samsung NZ63R3727BK service-parts table includes Midea-numbered 180 mm and 280 mm coil groupware. Applicability must be checked against the exact model/revision. [Service manual](https://www.manualslib.com/manual/2506905/Samsung-Nz63r3727bk.html)

## Data-quality rules

1. **No guessed values.** Blank means not found in reviewed source material; it does not mean zero.
2. **Keep measurement conditions.** For L/R/Q, record frequency, instrument/method, cookware, pan dimensions/material and coil-to-pan spacing. A number without conditions may be misleading.
3. **Keep source class visible.** Vendor datasheet, vendor manual graph, retailer listing, service manual and academic paper are different evidence classes.
4. **Don't merge aliases automatically.** Similar diameters, generic pictures, cross-brand compatibility lists or retailer claims do not prove identical winding/ferrite/sensor construction.
5. **Distinguish coil from complete assembly.** A coil cassette may include ferrite, temperature sensor, supports, wiring and connectors. Record which are included.
6. **Distinguish nominal appliance power from coil limits.** Retailer wattage often describes the intended appliance/zone, not a transferable electrical rating.
7. **Keep exclusion records when helpful.** Generic search results also return radiant hotplates; verify that each candidate is truly an induction coil before adding it to the candidate set.
8. **No compatibility claim from catalogue data alone.** Only a documented and validated coil + resonant network + inverter + control + mechanical stack-up can be approved for PawPlate.

## Recommended next collection pass

1. Expand the OEM spare-part inventory across major European spare-parts retailers and manufacturer service portals; capture exact part number, source price/stock snapshot, diameter/geometry, appliance compatibility and included sensor/ferrite details.
2. Search service manuals and exploded-parts lists for 140/145 mm, 160 mm, 180 mm, 210 mm and flexible/multi-zone assemblies; de-duplicate only when exact manufacturer part identity is established.
3. Prioritize supplier outreach for the Midea 17466000000118 and the Infineon 170 mm reference coil: request drawing, winding construction, inductance/impedance vs frequency and cookware, AC resistance/Q, current/temperature limits, sensor curve and connector details.
4. Add source publication date, retrieval date, currency, shipping/tax context and archived source snapshots when available.
5. Keep an explicit distinction between **catalogue completeness** (how many parts found) and **electrical completeness** (how well a given part is characterized).

## Important limitation

This is an initial web-discovered sample, not a claim to have found every coil assembly available worldwide. Stock and prices change. No item in this dataset is approved for use in PawPlate.
