# Power-stage topology comparison for PawPlate

Date: 2026-10-09  
Status: research comparison; recommendation for the next design phase, not an approved circuit.

## Decision in brief

**Carry the half-bridge series-resonant (HBSR) topology forward as PawPlate's primary candidate. Keep single-switch quasi-resonant (QR/SEPR) as a cost-focused fallback.**

Reason: PawPlate is a two-zone hob, and its priority is repairability, predictable module boundaries and a documented route to sharing a total power budget. Infineon's topology guide says QR is especially common in single-zone and cost-focused appliances, while half-bridge is commonly used in multi-zone appliances. It describes QR's switch-voltage sensitivity to load and mains disturbances, while HBSR's switch voltage is constrained by the DC bus and power control is more straightforward through switching-frequency adjustment. The half-bridge costs more and uses more board area/components.

This is a research preference, not a final engineering sign-off. The actual coil, inverter channel, control strategy, EMC and thermal design must be validated together.

## Evidence and references

1. Infineon, *Reverse-conducting IGBTs for induction cooking and resonant applications*, AN 2014-01, version 3.01, 2021-08-24. Its topology overview identifies QR as common in single-hob/cost-sensitive appliances and HBSR as common in higher-end multi-zone appliances. It lists QR advantages as a single switch, low cost and soft-switching efficiency, but flags high resonant switch voltage, sensitivity to grid/load changes, load-dependent frequency, and limits at high power and very low continuous power. It lists HBSR benefits including good efficiency over a wide power range, easier power control, and use of lower-voltage-class switches; drawbacks are cost, complexity and space.  
   https://www.infineon.com/assets/row/public/documents/60/42/infineon-an2014-01-reverse-conducting-igbt-applicationnotes-en.pdf

2. Infineon, *EVAL_2KW_SiC_IH*, application note, 2025-09-30. A documented 2 kW half-bridge series-resonant evaluation board with a 650 V SiC MOSFET, two gate-driver daughter-card options and a supplied resonant coil. The guide says it was tested up to 2 kW with a 320–340 V DC supply and 18 V auxiliary supply; it omits the mains rectifier/filter and auxiliary supply regulator stages. It is useful for studying modular driver boards and a replaceable test coil, but is not a complete mains-powered hob or a finished two-zone module.  
   https://www.infineon.com/assets/row/public/documents/24/42/infineon-eval-2kw-sic-ih-applicationnotes-en.pdf  
   https://www.infineon.com/evaluation-board/EVAL-2KW-SIC-IH

## Comparison

| Criterion | Single-switch quasi-resonant (QR/SEPR) | Half-bridge series-resonant (HBSR) |
|---|---|---|
| Main switch count per zone | One main switch (typically an induction-cooking IGBT with suitable reverse conduction) | Two main switches in a half-bridge |
| Component cost | Lower | Higher: more switches and a half-bridge driver |
| PCB/heatsink footprint | Typically smaller | Typically larger |
| Switch voltage stress | Resonant peak voltage can greatly exceed the bus and depends on load/power; protection and margin are especially important | Each switch voltage is constrained by the DC bus in the documented topology, allowing lower voltage-class devices in common designs |
| Load variation | Resonant conditions and switching behaviour are sensitive to the coil/pan equivalent load | Control through switching frequency is more direct; resonant operation still depends on the load |
| Low continuous power | Can encounter hard-switching limits in some operating conditions | Source describes good efficiency across output levels, provided the control strategy preserves soft switching |
| Two-zone coordination | Possible, but load-dependent frequency and operating behaviour complicate coordination | More natural candidate for coordinating independently controlled channels; frequency coordination and acoustic noise still need engineering |
| Repairability | Fewer devices to replace, but fewer parts alone do not make the system easier to diagnose or safer | More parts per channel, but modular half-bridge and driver boards can make faults more local and documented |
| Published reference evidence | Strong manufacturer application guidance; cost-focused designs common | Strong manufacturer application guidance plus a documented 2 kW SiC evaluation board with replaceable driver daughter cards and test coil |
| Fit for PawPlate | Cost-focused fallback | **Preferred candidate for continued research** |

## What this means for modular design

Proposed boundary for further design work:

- **Shared input section:** inlet/protection, EMI filtering, rectification and DC link, subject to a later decision about one shared bus versus separate channels.
- **Zone A power channel:** half-bridge switch assembly, gate-driver daughter card, local resonant capacitor network, current/voltage sensing and documented fault interface.
- **Zone B power channel:** same interface and preferably the same approved component set as Zone A, if the power/thermal design permits.
- **Coil cassette:** replaceable coil + ferrite + mount + temperature sensor. It must be explicitly approved for the particular resonant network and inverter firmware/hardware revision.
- **Control module:** ESP32 controller supplies commands and supervises status; independent hardware fault circuitry must inhibit switching even if firmware stalls.
- **Service access:** make the fan, controller, UI and power channels removable without removing unrelated modules. Keep high-frequency resonant loops physically compact; do not route them through long generic harnesses merely to make modules separable.

The controller should request power in watts or a documented normalized power command, but the low-level power channel must enforce its own current/voltage/temperature/fault limits. The appliance-level scheduler enforces the 3.0 kW continuous target and 3.68 kW peak input ceiling across both zones and auxiliary loads.

## Decisions not made yet

1. Shared DC link or separate rectified supplies for each zone.
2. IGBT or SiC MOSFET device family. The Infineon 2 kW SiC board is evidence for a modern half-bridge approach, not proof that SiC is the best cost/reliability choice for PawPlate.
3. Coil part and resonant capacitor values.
4. Switching frequency range and power-control method.
5. Exact ESP32 variant and hardware PWM/fault architecture.
6. Enclosure, cooling, EMI and appliance safety validation.

## Next research task

Before selecting part numbers, investigate whether there are open schematics/BOMs or serviceable commercial subassemblies for a two-zone half-bridge hob, and obtain electrical data for at least one real coil assembly. Keep this phase non-destructive and research-first; do not disassemble the user's existing hob.
