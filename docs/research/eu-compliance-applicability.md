# EU/Germany compliance applicability screening

**Research date:** 2026-10-09  
**Status:** preliminary regulatory research for a hypothetical domestic built-in induction hob. This is not legal advice, a conformity assessment, or a declaration that PawPlate is compliant.

## Why this document exists

PawPlate is a concept for a stationary, two-zone, mains-powered domestic induction hob. Compliance planning must cover the complete finished appliance, not just the power PCB or the individual CE/RoHS claims of purchased components. Applicable legal acts, harmonised EN standards, German implementation details and assessment routes must be re-checked for the exact product and intended market before any physical product is placed on the market.

## Preliminary applicability matrix

| Area | Likely relevance to a finished PawPlate appliance | Research status / next action |
|---|---|---|
| Electrical safety — Low Voltage Directive 2014/35/EU | **Expected to apply.** The LVD scope covers 50–1,000 V AC or 75–1,500 V DC; a 230 V mains hob is within those voltage limits. | Build a safety plan around the finished appliance. Confirm the current consolidated directive and applicable harmonised EN standards from the Official Journal at the time of assessment. |
| Household cooking-appliance safety — EN/IEC 60335 family | **Core standards family.** IEC 60335-2-6:2024 explicitly covers stationary household cooking appliances including induction hobs, in conjunction with IEC 60335-1. | The international IEC edition is a useful scope reference, not proof that this exact edition is the harmonised EN edition applicable in the EU. Verify the current EN edition/amendments and its Official Journal citation. Obtain qualified standards review and a complete test plan. |
| Electromagnetic compatibility — EMC Directive 2014/30/EU | **Expected to apply.** High-frequency resonant switching, touch/UI electronics and mains input can create conducted/radiated emissions and immunity concerns. | Identify the correct product-family EMC standards and test configuration. Do not treat a quiet bench prototype or compliant component as proof of appliance-level EMC. |
| Candidate EMC / EMF standards | Standards worth checking include the current EN editions of the household-appliance EMC series (for example EN IEC 55014-1 and EN IEC 55014-2), applicable mains harmonic/flicker standards, and EN 62233 for electromagnetic fields around household appliances. | These are a research shortlist only. Confirm current scope, edition, amendments and harmonised status against the Official Journal and the exact appliance configuration. Do not infer harmonisation from a search result or a supplier declaration. |
| Ecodesign — Commission Regulation (EU) No 66/2014 | The European Commission identifies domestic hobs as in scope of ecodesign requirements, including energy-efficiency performance and product information. | Plan to measure and document efficiency/performance using the legally prescribed method and assess product-information requirements. Verify the current consolidated text and any amendments before commercialization. |
| Off / standby energy — Commission Regulation (EU) 2023/826 | The Commission identifies electric hobs and hot plates within this regulation's product coverage. It repealed Regulation 1275/2008 in May 2025. | Include off-mode and standby power in the architecture and test plan, including any always-on controller, touch interface, indicator, fan control or connectivity rail. Re-check the regulation and applicable transition provisions at product release. |
| RoHS — Directive 2011/65/EU and amendments | Likely relevant to electrical/electronic equipment, subject to the directive's scope and any applicable exemptions. | Maintain material declarations for purchased parts, PCB finishes, solder, cables, connectors and components. Confirm any exemptions and documentation duties for the finished appliance. |
| WEEE — Directive 2012/19/EU and German ElektroG | Likely relevant if the finished appliance is placed on the EU/German market. Producer registration, marking, reporting and take-back responsibilities may apply. | Treat this as a commercialization workstream, not a PCB design task. Confirm which entity would be the producer and the current German registration/labeling obligations before any sale or distribution. |
| REACH and materials obligations | Potentially relevant to substances in components, cables, plastics, coatings and supplied articles. | Collect supplier declarations and check applicable substance communication obligations for the final product and supply chain. |
| Radio Equipment Directive (RED) | **Conditional only.** Relevant if a radio function (for example Bluetooth or Wi-Fi) is built into the final product; not assumed for the current concept. | If radio is added, assess RED requirements and how they interact with safety/EMC obligations. A pre-certified radio module does not certify the complete appliance. |
| Product documentation and CE marking | The finished product requires an appropriate conformity assessment, technical documentation, risk assessment, traceability and required instructions/markings before lawful placement on the market. | Maintain a requirements-to-evidence matrix. CE marking is the manufacturer's declaration of conformity, not a generic third-party certificate and not something established by selecting CE-marked components. Confirm the applicable conformity-assessment procedure with qualified support. |

## Key primary sources

- [IEC 60335-2-6:2024 — stationary cooking ranges, hobs, ovens and similar appliances](https://webstore.iec.ch/en/publication/65424)
- [EU Low Voltage Directive 2014/35/EU — current consolidated version](https://eur-lex.europa.eu/eli/dir/2014/35/2026-05-30/eng)
- [EU EMC Directive 2014/30/EU — current consolidated version](https://eur-lex.europa.eu/eli/dir/2014/30/2026-05-30/eng)
- [European Commission: harmonised standards under the Low Voltage Directive](https://single-market-economy.ec.europa.eu/single-market/goods/european-standards/harmonised-standards/low-voltage-lvd_en)
- [Commission Regulation (EU) No 66/2014 — ecodesign for domestic ovens, hobs and range hoods](https://eur-lex.europa.eu/eli/reg/2014/66/oj/eng)
- [European Commission: hobs and ecodesign](https://energy-efficient-products.ec.europa.eu/product-list/hobs_en)
- [Commission Regulation (EU) 2023/826 — off-mode, standby and networked-standby energy consumption](https://eur-lex.europa.eu/eli/reg/2023/826/oj/eng)
- [European Commission: standby, networked standby and off mode](https://energy-efficient-products.ec.europa.eu/product-list/standby-networked-standby-and-mode_en)
- [EU RoHS Directive 2011/65/EU](https://eur-lex.europa.eu/eli/dir/2011/65/oj/eng)
- [EU WEEE Directive 2012/19/EU](https://eur-lex.europa.eu/eli/dir/2012/19/oj/eng)

## Important caution about harmonised standards

The Commission's LVD harmonised-standards page notes that its summary list has not been updated from April 2025 onwards because of maintenance work. Therefore, do not use the summary list alone as a definitive current list. Check the relevant implementing decisions and amendments published in the Official Journal for the date and product scope under review.

Also distinguish the IEC international publication from an EN adoption and from a harmonised standard citation. A newer IEC edition does not automatically mean that edition is the currently harmonised European route to presumption of conformity.

## Requirements-to-evidence structure for future work

Before any compliance claim, create a controlled table with at least these columns:

- Requirement / hazard
- Applicable legal act or standard clause (exact edition and amendment)
- Design feature or control addressing it
- Verification method and acceptance criterion
- Test setup and required instrument
- Test result, date and responsible reviewer
- Evidence file / report revision
- Deviations, corrective action and closure evidence

At minimum, the future risk and verification work must consider electric shock, insulation and protective earthing, leakage current, abnormal operation, overcurrent and overtemperature, residual heat, cookware detection, unsuitable/empty cookware, glass and mechanical hazards, cooling failure, fire, EMC, electromagnetic fields, off/standby energy, repair/service access and foreseeable misuse.

No tests have been performed for PawPlate. This matrix is an initial planning aid only, and must not be read as a complete list of legal obligations or proof of compliance.
