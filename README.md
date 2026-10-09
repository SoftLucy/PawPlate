# PawPlate 🐾

An open-source, repairable two-zone built-in induction cooktop, inspired by affordable domino hobs.

## Project status

**Concept and research only.** This is a fun, exploratory thought experiment - not a validated product or a promise that a working hob will be built. No hardware has been built or validated.

### AI-assisted work - human review required

This project is being developed **heavily with AI assistance**. AI-generated research, diagrams, code, calculations, component suggestions, and documentation can be wrong, incomplete, outdated, or misleading. **Do not treat anything in this repository as trustworthy or verified just because it is written here. Everything must receive human review and independent verification before it is relied on**, especially any electrical, thermal, mechanical, firmware, sourcing, or safety claim. Links and citations should also be checked against their original sources.

Mains-powered induction circuitry can cause fatal electric shock, fire, or other serious harm. Nothing here is a build-ready design. Any physical implementation would require competent human engineering, qualified review, suitable test facilities, and appropriate safety validation.

## Goals

- Explore a compact, two-zone built-in hob comparable to a budget 3.4 kW domino cooktop.
- Explore reproducible hardware, firmware, mechanical design, documentation, and cost estimates.
- Prioritize repairability, documented interfaces, and readily available components.
- Investigate power-sharing architectures and cost reduction through research and simulation before hardware spending.
- Keep the initial project achievable with a zero-budget, documentation-first workflow.

## Planned documentation

- `docs/project-goals.md` - scope and success criteria
- `docs/reference-appliance.md` - non-destructive notes about the reference hob
- `docs/system-architecture.md` - candidate architecture and open decisions
- `docs/safety-requirements.md` - safety considerations and validation needs
- `docs/cost-model.md` - target cost and bill-of-materials assumptions
- `docs/research/` - references, application notes, and prior art
- `decisions/` - architecture decision records

## Contributing and forking

You are welcome to explore the idea, fork the repository, open issues, suggest corrections, or contribute research and documentation. You do not need to be an electrical engineer to help with tasks such as checking sources, improving explanations, documenting parts, or reviewing repairability ideas. Please be respectful and make uncertainty clear.

A simple way to contribute on GitHub:

1. **Fork** the repository to create your own copy under your GitHub account.
2. Make your changes in your fork, ideally on a separate branch.
3. **Open a pull request** from your branch to this repository and describe what you changed, why, and which sources support it. A pull request is a proposal for review; it does not automatically mean the change will be accepted.
4. Alternatively, open an **issue** to report a problem, ask a question, or suggest an idea without changing files.

For research contributions, include original sources and access dates where useful. Mark assumptions clearly and distinguish among AI-generated suggestions, independently checked claims, simulation results, and physically tested results. Please do not submit unverified AI output as established fact. All contributions need human review; safety-critical proposals need appropriate specialist review.

GitHub's guides explain the workflow: [Fork a repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo) | [Contributing to projects](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project) | [About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests).

## Safety and maturity

This repository is not a construction guide for a working mains-powered appliance. Any future power-electronics design will require qualified review, appropriate test equipment, and validation against applicable appliance and electrical-safety requirements. Simulation, citations, AI assistance, and documentation do not establish safety.

## License

Licensing has not yet been selected. Hardware and software may use separate licenses; see `LICENSES.md` for the planned decision. Until a license is chosen and added, do not assume the repository grants permission to reuse or redistribute all of its contents; GitHub's [licensing guidance](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository) explains why a license matters.


## Coil assembly research

- `docs/research/coil-assembly-comparison.md` - sourced comparison of commercially listed replacement coils, documented evaluation-board coil assemblies, service-manual examples, and published numerical coil characteristics. Missing specifications are marked unknown; no coil is approved for use.
