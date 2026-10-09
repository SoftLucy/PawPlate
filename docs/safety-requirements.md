# Safety requirements (initial checklist)

**Status:** early planning checklist and preliminary requirements; not a complete risk assessment, safety case, or validated design.

PawPlate is a concept only. No hardware has been built or validated. Mains and the DC link must be treated as potentially lethal. No requirement below defines circuit values or a build-ready implementation; detailed design and verification require qualified engineering review and appropriate laboratory facilities.

## Traceable preliminary requirements

| ID | Requirement intent | Design evidence expected later | Verification evidence expected later | Open items |
|---|---|---|---|---|
| SAFE-001 | Power-stage enable shall default to the non-switching state during power-up, reset, brownout, controller crash and loss of controller power. | Defined hardware default-off behaviour and documented interface between controller, driver and power stage. | Review and test each startup/reset/brownout/fault scenario with the power stage safely instrumented. | Exact architecture, state polarity, timing and component behaviour. |
| SAFE-002 | A dangerous power-stage fault shall be able to inhibit switching without waiting for an application task, scheduler, display, network stack or normal firmware execution. | Independently analysed hardware fault path and gate-disable behaviour; MCU fault reporting is supplementary. | Fault-injection test plan demonstrating the inhibit path and defined safe state. | Fault classes, thresholds, propagation timing and independent failure analysis. |
| SAFE-003 | Overcurrent, DC-bus overvoltage, undervoltage where relevant, overtemperature and gate-driver supply faults shall have defined responses. | Hazard-to-protection mapping, tolerances, diagnostics and safe recovery policy. | Analysis and tests over operating extremes and component tolerances. | Actual limits, sensing circuits, reaction times and reset conditions. |
| SAFE-004 | A fault shall not cause uncontrolled automatic re-energization. Restart behaviour shall be explicitly defined for each fault class. | State machine and fault-latching/recovery policy. | Test fault assertion, clearing, reset, brownout and repeated-fault cases. | Which faults require user acknowledgement or power cycling. |
| SAFE-005 | Failure or implausible readings from current, voltage, coil-temperature and other safety-relevant sensors shall be detected where practicable and lead to a defined safe response. | Sensor fault model including open circuit, short circuit, out-of-range, stuck signal and connector loss. | Fault injection and plausibility checks. | Sensor selection, thresholds, diagnostic coverage and response timing. |
| SAFE-006 | Cookware presence and suitability shall be assessed before and during heating; absent or unsuitable cookware shall not lead to uncontrolled operation. | Defined pan-detection strategy and operating-state rules, including behaviour when cookware is removed. | Test matrix covering suitable/unsuitable cookware, removal, movement and boundary cases. | Detection method, acceptable cookware range and detection thresholds. |
| SAFE-007 | Each zone and the appliance as a whole shall remain within the defined total input-power budget, including auxiliary loads. | Power-allocation model with zone priorities, measurement assumptions, error margin and behaviour when sensing is invalid. | Verify commanded and measured total input across zone combinations, transitions and fault conditions. | Final continuous/peak limits and the installation assumptions. |
| SAFE-008 | Cooling loss, blocked airflow, abnormal temperatures and thermal sensor faults shall not create an uncontrolled hazard. | Thermal design, airflow requirements, monitoring strategy and derating/shutdown policy. | Thermal testing and fault scenarios across intended ambient and load conditions. | Thermal limits, airflow, fan diagnostics and component derating. |
| SAFE-009 | Mains isolation, protective earthing, insulation coordination, creepage/clearance, leakage current and stored-energy hazards shall be addressed at the complete-appliance level. | Qualified electrical-safety design review and controlled construction documentation. | Appropriate safety tests, including discharge verification and abnormal conditions, performed by competent personnel. | Applicable standard edition, installation class, enclosure and detailed construction. |
| SAFE-010 | Loss of communication between the power-control module, supervisory controller and UI shall not defeat protective behaviour or leave the appliance in an undefined state. | Interface contract, timeout behaviour, safe defaults and state ownership. | Disconnect, corrupted-message, reset-order and stale-command tests. | Communication protocol and ownership of safety-relevant state. |
| SAFE-011 | The design shall address residual heat, hot surfaces, glass/ceramic breakage, mechanical support, liquid ingress/contamination, fire and foreseeable misuse. | Product-level hazard analysis covering electrical, thermal, mechanical and user-interface risks. | Applicable appliance-standard tests and documented risk-reduction review. | Mechanical stack, materials, enclosure, indicators and foreseeable misuse cases. |
| SAFE-012 | EMC and electromagnetic-field exposure shall be assessed on the complete appliance, with no assumption that component-level compliance is sufficient. | EMC/EMF test plan, layout/grounding strategy and configuration control. | Emissions, immunity and applicable EMF measurements in representative operating modes. | Applicable product-family standards, test levels and final configuration. |
| SAFE-013 | Off-mode and standby energy use shall be measured and controlled without compromising safety. | Defined standby/off states and rail/power policy. | Measurement under prescribed conditions and operating modes. | Applicable ecodesign/standby limits and any connectivity requirements. |
| SAFE-014 | Serviceability shall not create an unsafe state: modules, connectors, coil assemblies and covers need defined handling and reassembly requirements. | Service instructions, keyed interfaces, inspection criteria and controls against incorrect assembly. | Service/reassembly review and relevant mechanical/electrical tests. | Connector family, retention, service access and revision control. |

## Initial checklist

- Identify applicable EU/German electrical, EMC, and household cooking-appliance requirements.
- Treat mains and the DC link as potentially lethal; define isolation boundaries and safe discharge verification.
- Design independent overcurrent, overvoltage, and overtemperature protection appropriate to the selected topology.
- Address cookware detection, empty-pan/dry operation, abnormal loads, sensor faults, and safe fault shutdown.
- Assess protective earthing, insulation, creepage/clearance, leakage current, EMI, fire containment, glass/thermal hazards, and cooling failure.
- Define test procedures and required equipment before energizing any prototype.
- Obtain qualified review and formal verification before any appliance is used for cooking or offered to others.

## Regulatory research reference

See `docs/research/eu-compliance-applicability.md` for a preliminary EU/Germany applicability screening and authoritative links. It is a research aid, not a complete legal assessment. Verify the exact current EN editions and harmonised status before any conformity claim.

## Evidence and change control

For each requirement, future work should record the requirement revision, hazard/rationale, implementation reference, verification method and acceptance criteria, test setup, result, reviewer, evidence-file revision, deviations and closure. Do not mark a requirement verified based only on simulation, a reference design, a vendor claim or AI-generated text.

Never describe a simulated, unreviewed design as safe or compliant.
