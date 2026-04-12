# FLUX ISA Semantic Gap Analysis — Round 1

## Author
DeepSeek-Reasoner vessel, Cocapn fleet — 2026-04-12

## Context
The FLUX conformance suite has 113/113 passing test vectors. This analysis identifies behaviors NOT covered by those tests — semantic gaps where the specification is ambiguous, untested, or potentially dangerous.

## Gaps Found

### 1. DIV by Zero / INT_MIN Edge Case
- **Untested**: DIV when divisor converts to 0 or INT_MIN (-2^31) as double has rounding issues
- **Impact**: Undefined behavior in real deployment
- **Test**: PUSH INT_MIN_double, PUSH 0, DIV

### 2. MOD with Negative Divisor
- **Untested**: Euclidean modulo with negative divisor (-5 % -3)
- **Impact**: Result sign semantics may differ between implementations
- **Test**: PUSH -5, PUSH -3, MOD

### 3. MUL Carry Flag
- **Untested**: MUL producing result > 0xFFFFFFFF but fitting in double mantissa
- **Impact**: Carry flag semantics for multiplication unclear
- **Test**: PUSH 0x10000, PUSH 0x10000, MUL (produces 0x100000000, carry=1)

### 4. SHL/SHR with Non-Integer Shift Count
- **Untested**: Shift by 3.5 instead of 3
- **Impact**: Implementation-dependent truncation behavior
- **Test**: PUSH 1, PUSH 3.5, SHL

### 5. POKE Unaligned Address
- **Untested**: POKE to address 1 (not 4-byte aligned)
- **Impact**: Byte ordering confusion, potential data corruption
- **Test**: PUSH 1.0, PUSH 0x12345678, POKE

### 6. Float Special Values (NaN, ±Inf)
- **Untested**: FSUB(+inf, +inf) = NaN, FDIV(0, 0) = NaN
- **Impact**: NaN propagation through confidence and comparison ops
- **Test**: PUSH +inf, PUSH +inf, FSUB

### 7. CONF_MUL with NaN
- **Untested**: CONF_MUL when confidence is NaN from prior operation
- **Impact**: [0,1] clamp doesn't catch NaN; confidence becomes permanently invalid
- **Test**: CONF_GET, PUSH NaN, CONF_SET, PUSH 0.5, CONF_MUL

### 8. JZ/JNZ Without Prior Comparison
- **Untested**: ADD sets flags, then JZ uses stale FLAGS.Z
- **Impact**: Control flow depends on implementation-defined flag persistence
- **Test**: PUSH 5, PUSH -5, ADD, JZ addr (5 + -5 = 0, Z flag set by arithmetic)

### 9. A2A Queue Full
- **Untested**: SIGNAL when queue is at capacity
- **Impact**: Drop vs block vs overwrite — critical for real-time systems
- **Test**: Loop 1000x PUSH 1, SIGNAL channel 0

### 10. RET with Empty Call Stack
- **Untested**: RET without matching CALL
- **Impact**: Control flow hijack potential
- **Test**: RET as first instruction

### 11. Denormal Float Results
- **Untested**: FADD producing denormal numbers
- **Impact**: Hardware-dependent flush-to-zero behavior
- **Test**: PUSH 1e-308, PUSH 1e-308, FADD

### 12. Memory Aliasing (STORE/PEEK)
- **Untested**: STORE at addr X, then PEEK at addr X+1
- **Impact**: Different byte ordering at non-aligned offsets
- **Test**: PUSH 1.0, PUSH 0x1000, STORE, PUSH 0x1001, PEEK

## Critical Patterns
1. **Double/Integer representation mismatches** — dual use of doubles creates subtle conversion issues
2. **Flag persistence semantics** — FLAGS behavior across non-flag-setting instructions undefined
3. **Memory model gaps** — alignment, aliasing, out-of-bounds not specified
4. **IEEE 754 special values** — NaN, ±inf, denormals untested in most contexts
5. **Queue/Stack boundary conditions** — empty/full conditions for all FIFOs/LIFOs

## Cost
- Model: deepseek-reasoner
- Tokens: 7,745 (6,246 reasoning, 1,499 content)
- Duration: ~60s

## Connections
- [flux-conformance-runner](https://github.com/Lucineer/flux-conformance-runner) — 113/113 tests
- [flux-runtime-c](https://github.com/Lucineer/flux-runtime-c) — C runtime implementation
- [cuda-instruction-set](https://github.com/Lucineer/cuda-instruction-set) — full opcode catalog
