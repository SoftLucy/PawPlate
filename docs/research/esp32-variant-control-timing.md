# ESP32-family controller feasibility screen (variant undecided)

Date checked: 2026-10-09

## Summary

The project decision is **ESP32 family only**; no specific chip or controller partition has been selected. Current Espressif documentation shows meaningful differences between variants: ESP32-S3 explicitly lacks MCPWM ETM events, while ESP32-C6 and ESP32-P4 document MCPWM ETM support. Both C6 and P4 datasheets list ADC and MCPWM among ETM-capable peripherals. P4 offers two 12-bit SAR ADCs and continuous sampling/DMA; C6 has a 160 MHz high-performance RISC-V core, a low-power core, a 12-bit SAR ADC and MCPWM/ETM peripherals. C6 also has published ADC errata on older silicon revisions, including ADC/GDMA duplicate-data behaviour under a documented clock condition and a loss-of-precision issue; the errata page says ADC-305 is fixed in revision v0.2. These headline features are only a shortlist filter, not a suitability ranking, and exact silicon revision matters.

ESP32-P4 documents a flexible MCPWM event comparator that can mark an arbitrary PWM phase, and its technical reference manual lists ADC ETM tasks including ADC_TASK_START for high-performance ADC multi-channel sampling. ESP32-C6 also documents MCPWM ETM and lists ADC and MCPWM as ETM-capable. ESP32-S3 explicitly lacks MCPWM ETM events. However, the high-level ADC driver pages do not establish a supported public API and complete driver-mode configuration for phase-locked acquisition on either C6 or P4; an ADC_TASK_START event may start a sampling run rather than yield exactly one conversion per PWM event. The hardware feature matrix therefore does not select a variant. All candidates require target-specific review of exact silicon, ESP-IDF version, peripheral resources, event-to-sample relationship, timing and analog performance. The retrieved P4 datasheet/TRM are preliminary; verify current revisions and errata.

## Official-source comparison

| Capability | ESP32-S3 | ESP32-C6 | ESP32-P4 | PawPlate implication |
|---|---|---|---|---|
| MCPWM resources and ETM | Two MCPWM groups in older official docs; ETM events explicitly unsupported | One MCPWM group; ETM supported | Two MCPWM groups; ETM and dedicated event comparator supported | C6 may be resource-constrained for two independent zones; P4 and S3 have two groups, but S3 lacks the documented ETM event route. Confirm current target-specific resource counts and allocation. |
| ADC capability | Continuous ADC with DMA; ESP-IDF documents target-specific limitations | 12-bit SAR ADC; ETM listed in datasheet; exact supported acquisition path needs review | Two 12-bit SAR ADCs; continuous conversion and GDMA; ETM listed in datasheet | Channel count, analog range, noise, throughput, and acquisition mode must be compared against the sensing needs; resolution alone is not a meaningful selection criterion. |
| PWM-synchronous ADC trigger | No documented direct MCPWM ETM route on S3 | MCPWM ETM documented; ADC task/software path not established by high-level driver docs | MCPWM ETM and ADC ETM tasks documented at hardware level; public driver configuration and one-event/one-sample semantics remain unclear | Do not choose based on peripheral lists. Verify supported API/mode, exact trigger-to-sample behaviour, jitter, conversion latency and loop performance. |
| Independent safety shutdown | MCU peripheral fault handling is not a complete appliance safety mechanism | Same | Same | External hardware protection remains mandatory for any variant. |

### Sources

- Espressif, [ESP32-S3 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/mcpwm/mcpwm_etm.html) — explicitly states that ESP32-S3 does not support MCPWM ETM events.
- Espressif, [ESP32-C6 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-reference/peripherals/mcpwm/mcpwm_etm.html) and [ESP32-P4 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/mcpwm/mcpwm_etm.html) — both describe hardware event routing; P4 specifically documents a dedicated event comparator that can be used as an ADC timing marker.
- Espressif, [ESP32-C6 datasheet](https://documentation.espressif.com/esp32-c6_datasheet_en.html) — lists MCPWM and ADC among ETM peripherals, a 12-bit SAR ADC, and 160 MHz high-performance plus 20 MHz low-power RISC-V cores.
- Espressif, [ESP-IDF SoC capabilities for ESP32-C6](https://docs.espressif.com/projects/esp-idf/en/v5.5/esp32c6/api-reference/system/soc_caps.html) — documents one independent MCPWM group.
- Espressif, [ESP-IDF SoC capabilities for ESP32-P4](https://docs.espressif.com/projects/esp-idf/en/v5.3/esp32p4/api-reference/system/soc_caps.html) — documents two independent MCPWM groups and support for event comparators.
- Espressif, [ESP32-C6 Series SoC Errata](https://documentation.espressif.com/esp-chip-errata/en/latest/esp32c6/index.html) — documents ADC-305 (duplicate DMA data on affected older revisions under a clock condition; fixed in v0.2) and ADC-1477 (loss of precision in the lower four ADC bits). Check the exact silicon revision and applicable workaround before any variant comparison.
- Espressif, [ESP32-P4 datasheet](https://documentation.espressif.com/esp32-p4_datasheet_en.html) — documents 50 ETM channels, lists MCPWM and ADC among supported peripherals, and describes two 12-bit SAR ADCs, continuous transfer via GDMA, threshold monitors and analog voltage comparators.
- Espressif, [ESP32-P4 technical reference manual (pre-release PDF)](https://documentation.espressif.com/esp32-p4_technical_reference_manual_en.pdf) — the ADC-controller chapter lists ETM tasks including ADC_TASK_SAMPLE (LP ADC one-shot sampling), ADC_TASK_START (HP ADC multi-channel sampling) and ADC_TASK_STOP.
- Espressif, [ESP32-P4 ADC continuous-mode driver](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/adc/adc_continuous.html) — describes high-speed continuous sampling and DMA delivery.

## Design consequence

Keep the project-level decision at **ESP32 family, exact variant undecided**. ESP32-C6 and ESP32-P4 are candidates for further control-timing evaluation because their documentation lists MCPWM ETM support; ESP32-S3 remains another family member but lacks that particular feature and could only be assessed against a different control partition. No variant is preferred or selected by this screen. Do not assume any ESP32 is the sole safety controller.

Before making a variant decision, a qualified engineering review and low-risk test plan should establish at least:

1. Which ESP32 variant(s) are practical to evaluate, and whether the exact silicon revision and ESP-IDF version support the needed MCPWM-to-ADC ETM connection in the selected operating mode.
2. Whether the ADC trigger is accepted in the required conversion mode, and the actual trigger-to-sample timing and jitter.
3. Conversion latency, sample aperture, usable bandwidth, noise and calibration across the required analog input range.
4. Whether the necessary two-zone timer/operator resources can be allocated simultaneously without conflicting with other peripherals; current docs indicate one MCPWM group on C6 versus two on P4, so this is an important architecture constraint, not just a clock-speed comparison.
5. How PWM updates, missed samples, DMA overflow, CPU overload, watchdog resets and startup/reset states are detected and handled.
6. That independent external hardware can inhibit the power stage regardless of MCU firmware, clock state or power state.

This is a peripheral-feasibility screen only. It does not establish a validated induction-control algorithm or a safe power-stage design.
