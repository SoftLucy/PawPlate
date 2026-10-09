# Open reference designs and licensing survey

Date checked: 2026-10-09

## Purpose and boundary

PawPlate needs public engineering references, but a downloadable schematic, firmware package, or Gerber archive is not automatically open-source. This survey distinguishes material suitable for technical study from files whose license clearly permits modification and redistribution. It does not provide legal advice. No third-party design files or source code have been copied into PawPlate.

## Findings

### 1. Infineon Smart Induction Cooktop Pack — do not incorporate

Pack page: https://softwaretools.infineon.com/tools/com.ifx.tb.tool.modustoolboxpacksmartinductioncooktop

The pack advertises hardware Gerbers, application firmware, and BSPs for inverter-control, HMI/system-control, and connectivity boards. Access is login/request gated.

The Cypress (an Infineon company) EULA displayed on the pack page grants narrow rights. Section 2 permits modifying and compiling source firmware for execution on Cypress hardware, but permits distribution of firmware only in binary form and only when installed on Cypress hardware. Section 4 restricts copying and derivative works and says source code must be kept confidential. Section 3 says separately licensed third-party open-source components are governed by their own licenses.

**Decision:** Do not import its firmware, BSP, Gerbers, or other files into PawPlate, and do not redistribute them, unless the specific file has a separate license that clearly permits the intended use or Infineon gives written permission. The public user guide may still be cited as a technical reference; cite facts and link to the original rather than copying protected files.

### 2. Toshiba RD206 — technically relevant, not established as open hardware

Reference page: https://toshiba.semicon-storage.com/us/semiconductor/design-development/referencedesign/detail.RD206.html

Toshiba publishes a 2 kW, 200–240 V AC voltage-resonant soft-switching induction-cooker inverter reference. Its page lists a reference guide, design guide, sample software, circuit diagram, BOM, PCB data, and fabrication data in several EDA formats.

The page's availability of editable files does not itself grant an open-hardware license. The linked Toshiba site terms include site/materials conditions and disclaimers, but this research did not identify a clear permissive license authorizing PawPlate to redistribute or adapt those files.

The page also flags the 2SC6135 as not recommended for new designs as of April 2026. Confirm the exact design revision and all fitted parts before using any component claim.

**Decision:** Technical reading and citation only for now. Do not copy the CAD, PCB, sample software, or manufacturing files into this repository unless an applicable license or written permission is confirmed.

### 3. Renesas AS048-EVK — technically relevant, license still unresolved

Reference page: https://www.renesas.com/en/design-resources/reference-designs/as048-evk

Renesas describes a 2.1 kW single-burner induction-cooktop design using the iW248 smart IGBT driver/controller, with fast current/voltage-related protection features, temperature sensing, and an RL78/G15 HMI. A design-files link is provided.

The public page reviewed for this survey did not establish a license granting redistribution or derivative-work rights for all design files.

**Decision:** Keep as a technical reference. Treat downloads as proprietary/permission-unclear until their actual terms are checked; do not mirror them or build PawPlate files from copied CAD/code.

### 4. WEkigai Precius — clearly licensed, but not an induction power-stage design

Repository: https://github.com/WEkigai/Precius

Precius explicitly separates its licenses:
- Software code: GPL-3.0, subject to any additional licenses identified by individual modules.
- Hardware design information: CERN-OHL-S.
- Non-code files and creative assets: CC BY-SA 4.0, unless otherwise specified.

The project documents an ESP32-S3 controller, user interface, temperature sensing, repairability, and an open design process. However, the described Precius Station uses a **resistive heating element**, not an induction coil/inverter. Its repository says AC-power PCB work is still in progress for the referenced prototype stage.

**Decision:** A useful example of explicit, per-category licensing and modular cooking-appliance design. It is not evidence that the ESP32-S3 is suitable for resonant induction inner-loop control, and it is not a compatible induction power-stage design to adopt.

### 5. ReactorForge — open induction-heating reference, different application

Repository: https://github.com/ThingEngineer/ReactorForge

The repository identifies its plans, schematics, and code as CC BY-SA 4.0. It is a high-power induction-heating platform aimed at applications such as metalworking, not a domestic cookware appliance. Its power stage, work coil, load behaviour, user protection, electromagnetic environment, and duty cycle cannot be assumed equivalent to a household hob.

**Decision:** Useful for studying how an open project publishes source design material and for broad induction-heating concepts. Do not treat it as a validated hob design or copy its power stage without a separate, qualified engineering assessment.

## Licensing implications for PawPlate

The current PawPlate LICENSES.md correctly records that no project license has been selected. Keep that status until the project owner chooses explicit licenses and adds the corresponding license texts.

A possible license structure to evaluate (not yet selected):

| Content type | Candidate | Main consideration |
|---|---|---|
| Hardware source files (schematics, PCB layout, mechanical CAD) | CERN-OHL-S v2 or CERN-OHL-P v2 | Strongly reciprocal vs permissive hardware reuse |
| Firmware and software | GPL-3.0-or-later or another explicit software license | Derivative software and redistribution obligations |
| Documentation and original illustrations | CC BY-SA 4.0 or another explicit content license | Attribution and share-alike obligations |
| Third-party components, libraries, fonts, and assets | Their own upstream licenses | Must be inventoried; a repository-wide license cannot override them |

The CERN Open Hardware Licence v2 has three variants: strongly reciprocal (S), weakly reciprocal (W), and permissive (P). The choice should reflect whether PawPlate wants derivatives of its hardware designs to remain available under reciprocal terms or wants to maximize downstream reuse. This is a project-policy decision, not something to settle by copying another project's license without reviewing its obligations.

CERN OHL v2: https://cern-ohl.web.cern.ch/

## Research conclusion

No clearly licensed, directly reusable domestic induction-hob power-stage design was established by this survey. The closest public commercial reference designs are valuable technical documentation but have unclear or restrictive reuse terms. The most useful explicitly licensed projects found are adjacent rather than drop-in equivalents: Precius for repairable cooking-appliance architecture and licensing separation, and ReactorForge for open induction-heating documentation.

Continue with independent PawPlate documentation and simulation, cite manufacturer references, and keep external source files out of the repository unless their license is verified.
