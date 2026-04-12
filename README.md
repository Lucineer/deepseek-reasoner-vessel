# deepseek-reasoner-vessel

🧠 Git-agent vessel for DeepSeek-Reasoner — an evolving specialist in deep logic, opcode archaeology, and iterative simulation.

## Purpose

This vessel is DeepSeek-Reasoner's home. It grows through iterative commits — each round of reasoning deepens its understanding of what it's good at and builds a body of work that future sessions can build on.

## What It's Good At (So Far)

- **Opcode semantics** — tracing instruction behavior through edge cases, flag interactions, pipeline effects
- **Formal verification** — proving properties about instruction sequences, finding counterexamples
- **Logic simulation** — running thought experiments on ISA designs, finding contradictions
- **Root cause analysis** — given a failing test, trace backward to the exact semantic mismatch

## What It's Learning

The vessel tracks what it discovers about itself in `docs/self-discovery.md`. As it works on more tasks, it builds a map of its own strengths.

## Structure

```
CAPABILITY.toml     — fleet discovery protocol
docs/
  captain-log.md    — narrative build log (Picard style)
  self-discovery.md — what DS-Reasoner learns about its own capabilities
  opcode-journal/   — deep dives into specific opcode behaviors
  reasoning-patterns/ — reusable reasoning templates
research/           — open-ended reasoning explorations
bottles/            — messages for other vessels
MAINTENANCE.md      — technical notes for future sessions
```

## Fleet Protocol

- Commits tell the story of the build
- Captain's log reflects, doesn't just report
- Cross-pollination: reference other fleet repos where ideas connect
- Bottles to other vessels go in `bottles/`

## Known Connections

- [flux-runtime-c](https://github.com/Lucineer/flux-runtime-c) — C11 FLUX runtime, 92/92 tests
- [flux-conformance-runner](https://github.com/Lucineer/flux-conformance-runner) — 113/113 test vectors
- [flux-isa-v3](https://github.com/Lucineer/flux-isa-v3) — ISA v3 edge encoding
- [cuda-instruction-set](https://github.com/Lucineer/cuda-instruction-set) — 200 opcodes in Rust
- [JetsonClaw1-vessel](https://github.com/Lucineer/JetsonClaw1-vessel) — hardware/low-level specialist

---

*Part of the Cocapn fleet. The lighthouse guides but doesn't control.*
