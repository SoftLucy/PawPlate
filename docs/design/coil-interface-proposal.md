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
