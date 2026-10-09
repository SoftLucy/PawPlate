# ESP32 variant screen for PWM-synchronous sampling

Date checked: 2026-10-09

## Summary

The earlier ESP32-S3 screen found a real limitation: Espressif explicitly says ESP32-S3 does not support MCPWM ETM events. A follow-up check found that the **ESP32-P4 does document MCPWM Event Task Matrix (ETM) support**, and its datasheet lists both MCPWM and ADC among ETM-capable peripherals. Its ADC controller includes two 12-bit SAR ADCs, continuous sampling/DMA, and threshold monitors.

This makes ESP32-P4 a better **candidate to evaluate** for hardware-timed sampling than ESP32-S3. The ESP32-P4 technical reference manual documents ADC ETM tasks, including ADC_TASK_START to start HP ADC multi-channel sampling, while the MCPWM driver documents phase-positioned event comparators. That is evidence for a hardware-level route; however, ADC_TASK_START may start a sampling run rather than cause exactly one conversion per PWM event. The current high-level ADC driver page does not document a public ADC ETM-task API or fully explain the driver-mode semantics for this use. The exact software configuration, event-to-sample relationship, timing and suitability still need proof. The retrieved P4 datasheet and technical reference manual are preliminary; re-check current revisions, errata and target-specific IDF support before treating peripheral details as fixed. No MCU variant is selected by this note.

## Official-source comparison

| Capability | ESP32-S3 | ESP32-P4 | PawPlate implication |
|---|---|---|---|
| MCPWM ETM events | Explicitly unsupported | Documented, including timer/comparator events | P4 has a documented peripheral event path that S3 lacks. |
| ADC capability | Continuous ADC with DMA; ESP-IDF documents limitations including ADC2 DMA support | Two 12-bit SAR ADCs; continuous conversion and GDMA; threshold monitors | P4 has more explicit hardware acquisition/monitoring features, but accuracy, noise, input range and actual sample timing still need engineering validation. |
| PWM-synchronous ADC trigger | No documented direct MCPWM ETM route on S3 | P4 MCPWM ETM docs describe an event comparator as an ADC timing marker; the P4 technical reference manual lists ADC ETM tasks including ADC_TASK_START for HP ADC multi-channel sampling. The current high-level ADC driver page does not document a public API for configuring that ETM task | A hardware-level route appears possible, but ADC_TASK_START semantics do not establish one conversion per PWM event. Supported software configuration, event-to-sample relationship, jitter, conversion latency and loop performance remain unverified. |
| Independent safety shutdown | MCU peripheral fault handling is not a complete appliance safety mechanism | Same | External hardware protection remains mandatory, even if P4 is chosen. |

### Sources

- Espressif, [ESP32-S3 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/mcpwm/mcpwm_etm.html) — explicitly states that ESP32-S3 does not support MCPWM ETM events.
- Espressif, [ESP32-P4 MCPWM ETM documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/mcpwm/mcpwm_etm.html) — describes MCPWM timer/comparator events and notes that an event comparator can be used as an ADC timing marker.
- Espressif, [ESP32-P4 datasheet](https://documentation.espressif.com/esp32-p4_datasheet_en.html) — documents 50 ETM channels, lists MCPWM and ADC among supported peripherals, and describes two 12-bit SAR ADCs, continuous transfer via GDMA, threshold monitors and analog voltage comparators.
- Espressif, [ESP32-P4 technical reference manual (pre-release PDF)](https://documentation.espressif.com/esp32-p4_technical_reference_manual_en.pdf) — the ADC-controller chapter lists ETM tasks including ADC_TASK_SAMPLE (LP ADC one-shot sampling), ADC_TASK_START (HP ADC multi-channel sampling) and ADC_TASK_STOP.
- Espressif, [ESP32-P4 ADC continuous-mode driver](https://docs.espressif.com/projects/esp-idf/en/latest/esp32p4/api-reference/peripherals/adc/adc_continuous.html) — describes high-speed continuous sampling and DMA delivery.

## Design consequence

Keep the project-level decision at **ESP32 family**, but add ESP32-P4 as the leading variant for a control-timing proof of concept. Retain ESP32-S3 as a possible supervisory/UI option if the system is partitioned so it does not need PWM-phase-locked fast sampling. Do not assume that choosing P4 makes the ESP32 the sole safety controller.

Before making a variant decision, a qualified engineering review and low-risk test plan should establish at least:

1. Whether the exact silicon revision and ESP-IDF version support the needed MCPWM-to-ADC ETM connection in the selected operating mode.
2. Whether the ADC trigger is accepted in the required conversion mode, and the actual trigger-to-sample timing and jitter.
3. Conversion latency, sample aperture, usable bandwidth, noise and calibration across the required analog input range.
4. Whether the necessary two-zone timer/operator resources can be allocated simultaneously without conflicting with other peripherals.
5. How PWM updates, missed samples, DMA overflow, CPU overload, watchdog resets and startup/reset states are detected and handled.
6. That independent external hardware can inhibit the power stage regardless of MCU firmware, clock state or power state.

This is a peripheral-feasibility screen only. It does not establish a validated induction-control algorithm or a safe power-stage design.
