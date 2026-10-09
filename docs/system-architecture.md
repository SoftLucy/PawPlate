# System architecture (draft)

Candidate functional blocks:

1. Mains input, disconnect/protection, filtering, and power conversion
2. Resonant induction power stage(s) and coil assemblies
3. Voltage/current sensing and hardware fault shutdown
4. Supervisory controller and zone power scheduler
5. User interface and documented inter-module communication
6. Thermal management, enclosure, insulation, and protective earthing as applicable

## Open decisions

- Shared versus separate power stages for the two zones
- Power-sharing strategy and achievable simultaneous output
- Control and protection partitioning, including independent hardware shutdown
- Component availability, serviceability, and cost
- Applicable safety standards and verification plan

No topology has been selected. Evaluate candidates using published references, simulations, cost estimates, and qualified engineering review before physical implementation.
