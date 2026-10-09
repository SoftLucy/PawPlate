# PawPlate coil interface proposal

Date: 2026-10-09  
Status: proposed architecture; not a released specification and not proof of compatibility.

## Decision

PawPlate should be designed to support **multiple explicitly approved coil assemblies**, rather than assume one universal coil exists. The first prototype should support one fully characterized coil; additional coils can be added by publishing a new compatibility profile after engineering verification.

This is a project-defined interface. It is not an existing domestic induction-cooktop standard.

## Separate the physical interface from electrical compatibility

A coil can fit mechanically and still be unsafe or ineffective electrically. Therefore, compatibility has two independent gates:

1. **Mechanical and service interface:** dimensions, mounting, glass clearance, ferrite placement envelope, wiring retention, thermal-sensor connector and removal procedure.
2. **Electrical/thermal compatibility profile:** measured coil parameters, permitted operating range, approved resonant capacitor/power-stage combination, sensing calibration and test evidence.

A coil is approved only when both gates pass. Connector fit or an ID resistor is not sufficient evidence.

## Proposed replaceable coil cassette

Each cassette should contain:
- Coil winding and its fixed lead geometry
- Ferrite pieces and their retainers
- Mechanical support / mounting frame
- Temperature sensor(s) attached at defined positions
- A keyed, rated coil connection appropriate for the high-frequency current
- A separate keyed low-voltage sensor connector
- A durable label with manufacturer, exact part number and revision

The high-frequency connection should be physically short and mechanically secure. Do not use long generic extension wiring just to make the cassette removable: stray inductance, contact resistance, heating and EMI can change the behaviour of the resonant circuit. Connectors must be selected only after current, frequency, temperature, insulation and fault conditions are known.

## Proposed compatibility profile

Publish one file per approved coil model. Suggested fields:

| Field | Purpose |
|---|---|
| coil ID and revision | Stable identification; never identify a coil by diameter alone |
| Manufacturer / source / part number | Reorderability and provenance |
| Mechanical drawing and dimensions | Fit, mounting and glass spacing |
| Winding / ferrite construction | Identify relevant physical differences |
| Inductance and test conditions | Include frequency, instrument/setup and uncertainty |
| Winding resistance / test conditions | Include temperature and method where available |
| Sensor type and curve | Identify sensor model, nominal values and location |
| Approved inverter/channel revision | Bind the coil to the power stage it was validated with |
| Approved resonant network revision | Prevent incompatible capacitor substitutions |
| Allowed control profile | Limits are derived from validation, not guessed from coil size/wattage |
| Thermal and fault test references | Evidence for approval and future regression testing |
| Status | candidate, under-test, approved, withdrawn |
| Replacement notes | Required fasteners, leads, orientation and inspection steps |

Example skeleton (values intentionally omitted until measured):

```yaml
coil_id: vendor-part-revision
status: candidate
mechanical:
  drawing: docs/coils/vendor-part-revision.pdf
  mounting_revision: cassette-A
electrical:
  inductance: not-measured
  measurement_conditions: not-measured
temperature_sensor:
  part_number: unknown
power_stage:
  approved_channel_revision: none
  resonant_network_revision: none
validation:
  report: none
```

## Identification and fail-safe behaviour

A simple passive ID component may help identify a cassette, but it must not be used as the safety mechanism or as proof that the coil is compatible. The controller should:
- Refuse normal operation if the coil identity is missing, unknown or inconsistent with the selected hardware profile.
- Start only with a profile that has been explicitly approved for that hardware revision.
- Monitor sensor open/short conditions and plausible temperature change.
- Log the coil ID and power-stage revision in diagnostic information.

Independent hardware current/voltage fault shutdown remains required even if firmware recognizes the coil correctly. If identification circuitry fails, default to no heating.

## Compatibility levels

- **Level 0 — Mechanical fit only:** fits the cassette envelope; heating is not permitted.
- **Level 1 — Characterized:** mechanical, winding and sensor properties documented; heating still not approved.
- **Level 2 — Power-stage validated:** approved with one exact power channel and resonant-network revision after controlled testing.
- **Level 3 — Appliance validated:** approved in the complete glass, enclosure, cooling, sensing and control assembly after relevant safety/EMC/thermal testing.

Only Level 3 should be listed as a user-replaceable compatible coil in the finished product documentation.

## Practical development path

1. Find one candidate coil with credible documentation; record its part number and source.
2. Define the cassette envelope around real candidate hardware, rather than choosing arbitrary dimensions first.
3. Measure/obtain the electrical characteristics under controlled engineering conditions.
4. Match it to one documented power-stage and resonant-network revision.
5. Test the complete zone assembly with appropriate current/voltage instrumentation, thermal monitoring, fault tests and qualified review.
6. Add a second coil only if it can fit the same cassette envelope and pass the compatibility process.
7. Keep the coil profile and compatibility matrix version-controlled with the hardware BOM.

## Constraints

- Do not infer compatibility from nominal wattage, coil diameter, connector shape or a resistance reading alone.
- Do not swap coils on a live or energized appliance.
- Do not expose the user to live resonant-circuit measurements during ordinary service.
- Do not enable heating with an unknown coil or an unvalidated power-stage/coil combination.
- The mains and resonant power stage remain hazardous; this document is a requirements proposal, not a construction procedure.


## Required matching characteristics: compatibility rules

A replacement coil does not need to be dimensionally or electrically identical to the reference coil in every respect. It must, however, be demonstrated to remain inside the validated limits of the exact power stage, resonant network, firmware, sensor chain and mechanical stack-up. Until those limits are established, treat a candidate as unqualified.

### A. Must match the approved interface or stay within explicit limits

These characteristics are compatibility gates, not optional preferences:

- **Power-stage / resonant-network operating envelope:** loaded and unloaded impedance over the complete permitted switching-frequency range; resonant frequency and its variation with cookware and spacing; peak and RMS coil current; resonant capacitor voltage/current stress; switch voltage/current stress and soft-switching or other commutation conditions. These values must be compatible as a coupled system, not checked in isolation.
- **High-frequency losses and thermal limits:** coil AC resistance/Q versus frequency, conductor construction, allowable current and frequency, coil/ferrite temperature limits, and heat dissipation under worst validated conditions. DC winding resistance alone is not enough.
- **Insulation and electrical safety:** insulation system, creepage/clearance in the assembled appliance, lead insulation, connector ratings, strain relief, and separation from accessible/low-voltage parts must meet the appliance design's requirements.
- **Temperature sensing and protection:** sensor type/curve/tolerance, physical location and thermal coupling, connector pinout, open/short detection and over-temperature response must be explicitly supported by firmware and hardware. If the sensor differs, the sensing circuit and calibration must be validated for it.
- **Mechanical stack-up affecting electrical behaviour:** coil-to-glass and coil-to-pan distance, ferrite geometry/location, mounting rigidity and permitted movement must stay within the tested envelope. These alter magnetic coupling, impedance and heating.
- **Approved identity and revision:** exact manufacturer/part/revision must map to a compatibility profile and approved power-stage/resonant-network revision. A label or ID resistor only identifies a candidate; it does not prove safety or compatibility.

Infineon's EVAL-IHW25N140R5L manual explicitly says the supplied coil was used for board characterization and that load properties should be measured when another coil or cookware is used. Its plots show load resistance and inductance vary with frequency and vessel presence. This supports characterizing the *whole loaded resonant system*, rather than relying on diameter or nominal watts ([manual, Figure 16](https://www.infineon.com/dgdl/Infineon-EVAL-IHW25N140R5L-UserManual-v01_00-EN.pdf?fileId=8ac78c8c8afe5bd0018b18c78e023a8d)). Published induction-coil research also explains that high-frequency winding losses depend on skin and proximity effects and litz construction ([IEEE paper abstract](https://ieeexplore.ieee.org/document/4939678/)).

### B. May differ, but only inside validated limits

These are not required to be identical if the design has been intentionally engineered to accommodate them:

- **Inductance:** may differ only if measured under specified conditions and the full resonant/load envelope is compatible with the chosen inverter and capacitor network.
- **AC resistance/Q:** may differ only if loss, heating, power delivery, and switching behaviour remain within verified limits.
- **Diameter / shape / turns / litz strand details:** may differ if geometry, field distribution, current density, heating uniformity, EMC and thermal performance are validated. A similar diameter is not evidence of equivalence.
- **Ferrite material and geometry:** may differ only after validating magnetic losses, temperature, saturation margin, field distribution and nearby-component heating.
- **Rated zone power:** a printed wattage is not an intrinsic plug-in rating for a coil. The appliance's actual permitted power is a system-level result and must remain within the coil and inverter's validated envelope.
- **Sensor model:** may differ only with a compatible input circuit, documented curve/tolerance, calibration and fault handling.
- **Connector family:** may differ mechanically, but the approved cassette interface should use the specified keyed connector, contact rating, insulation and retention. Do not adapt an unknown connector by guesswork.

### C. Must match mechanically for a drop-in cassette

For the same PawPlate cassette slot, the mounting datum, allowed envelope, lead exit/length and routing, sensor connector/pinout, ferrite clearance, insulation layers and retention features must match the published drawing. If these change, it is a different cassette/mechanical revision and needs a fit/thermal/electrical review. The actual coil diameter may vary only if the cassette and validated cooking-zone geometry intentionally allow it.

### D. Minimum coil characterization record

Before a candidate can be electrically evaluated, record at minimum:

1. Exact part number/revision, source, drawing and construction photographs.
2. Inductance and impedance versus frequency using a documented method, both without cookware and with specified representative cookware and defined spacing.
3. Winding DC resistance at a stated temperature; AC resistance or Q over the intended operating band.
4. Coil RMS/peak current and relevant resonant-capacitor and switch stress for the candidate system, determined by qualified engineering analysis and appropriate tests.
5. Coil and ferrite temperature rise/losses at defined operating points and worst validated conditions.
6. Sensor type, curve/tolerance, location, thermal coupling and open/short behaviour.
7. Mechanical stack-up, insulation, connector/lead ratings and service procedure.
8. Test report tying the exact coil revision to the exact inverter, resonant-network, firmware/control and mechanical revisions.

No universal numeric acceptance limits are assigned here because PawPlate has not yet selected a power-stage topology, switching-frequency envelope, resonant network or sensor architecture. Limits must be derived for that design, not copied from a different vendor's evaluation board.

### E. Compatibility levels

- **Level 0 - Mechanical fit only:** fits the envelope; must not be energized on the basis of this status.
- **Level 1 - Characterized:** construction and impedance/sensor data recorded; still not approved for operation.
- **Level 2 - Power-stage validated:** exact inverter, resonant network, sensing and control tested by competent engineering against documented limits.
- **Level 3 - Appliance validated:** mechanical, thermal, EMC, abnormal/fault and relevant safety validation completed for the intended appliance configuration. Only this level may be described to users as PawPlate-compatible.
