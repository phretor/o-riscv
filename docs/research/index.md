---
hide:
  - navigation
  - toc
---
# Research

## RISC-V Fuzzing

|                                                                                   | [Cascade](https://github.com/cascade-artifacts-designs/cascade-meta) (2024) | [ProcessorFuzz](https://github.com/bu-icsg/ProcessorFuzz) (2023) | [hw-fuzzing](https://github.com/googleinterns/hw-fuzzing) (2022) | [DifuzzRTL](https://github.com/compsec-snu/difuzz-rtl) (2021) | [RFUZZ](https://github.com/ekiwi/rfuzz?tab=readme-ov-file) (2018) |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------------- |
| [VexRiscv](https://github.com/SpinalHDL/VexRiscv)                                 | 🟢 (7 CVEs)                                                                 |                                                                  |                                                                  |                                                               |                                                                   |
| [PicoRV32](https://github.com/YosysHQ/picorv32)                                   | 🟢 (6 CVEs)                                                                 |                                                                  |                                                                  |                                                               |                                                                   |
| [Kronos](https://github.com/SonalPinto/kronos/tree/master/rtl/core)               | 🟢 (6 CVEs)                                                                 |                                                                  |                                                                  |                                                               |                                                                   |
| [CVA6](https://github.com/openhwgroup/cva6)                                       | 🟢 (8 CVEs)                                                                 |                                                                  |                                                                  |                                                               |                                                                   |
| [BOOM](https://github.com/riscv-boom/riscv-boom)                                  | 🟢 (2 CVEs)                                                                 | 🟢                                                               |                                                                  | 🟢                                                            |                                                                   |
| [Rocket](https://github.com/chipsalliance/rocket-chip)                            |                                                                             | 🟢                                                               |                                                                  | 🟢                                                            | 🟢                                                                |
| [mor1kx](https://github.com/openrisc/mor1kx)                                      |                                                                             |                                                                  |                                                                  | 🟢                                                            |                                                                   |
| [BlackParrot](https://github.com/black-parrot/black-parrot)                       |                                                                             | 🟢                                                               |                                                                  |                                                               |                                                                   |
| [Sodor](https://github.com/ucb-bar/riscv-sodor)                                   |                                                                             |                                                                  |                                                                  |                                                               | 🟢                                                                |
| [OpenTitan](https://github.com/googleinterns/hw-fuzzing/tree/master/hw/opentitan) |                                                                             |                                                                  | 🟢                                                               |                                                               |                                                                   |

From a security standpoint, what makes RISC-V interesting is that the hardware definition of some IP cores is open source, which means that it can be formally verified and, best of all, fuzzed like software. The idea is simple:

- compile the core into an executable format,
- shovel it into a simulator,
- randomize inputs (i.e., instructions and data),
- observe execution,
- measure coverage,
- repeat,
- ...wait, what now?

The design and implementation of such a fuzzer are quite challenging, tho, mainly because, unlike with a traditional program, it's difficult to define an oracle that tells us when a bug in a CPU has been found, because the CPU just...interprets instructions based on state and data. It can't "crash". And if observe a crash, most likely it's the simulator crashing. We don't yet have a precise way to tell whether the CPU being simulated and fuzzed is behaving according to the specs, or maybe according to the best golden model we have so far. Even in this case, is the golden model representative? What are we trying to compare against? We don't (yet!) have the choice of memory or address sanitizers like we have for fuzzers. In other words, there's a huge body of research, development, and hacking waiting to be unfolded here. There exist the notion of coverage, which is good news.

I selected the following papers because (1) the evaluation is done on RISC-V cores or a full processor, and (2) they come with code, so the results are reproducible. In some cases, they're used to continuously fuzz the RTL of the cores, like in the case of the work by Trippel et al., which, according to the authors (and [this repository](https://github.com/googleinterns/hw-fuzzing)), their tool is used to fuzz Google's [OpenTitan](https://opentitan.org/).
## Papers with Tools

- Solt et al., [Cascade: CPU Fuzzing via Intricate Program Generation](https://comsec.ethz.ch/research/hardware-design-security/cascade-cpu-fuzzing-via-intricate-program-generation/), USENIX Security, 2024.
	- **Tool:** [Cascade](https://github.com/cascade-artifacts-designs/cascade-meta) (Docker)
	- **Target RISC-V:**
		- [VexRiscv](https://github.com/SpinalHDL/VexRiscv) (SpinalHDL)
		- [PicoRV32](https://github.com/YosysHQ/picorv32) (Verilog)
		- [Kronos](https://github.com/SonalPinto/kronos/tree/master/rtl/core) (Verilog)
		- [CVA6](https://github.com/openhwgroup/cva6) (Verilog)
		- [BOOM](https://github.com/riscv-boom/riscv-boom) (Chisel)
- Canakci et al., [ProcessorFuzz: Processor Fuzzing with Control and Status Registers Guidance](https://arxiv.org/pdf/2209.01789), IEEE HOST, 2023.
	- **Tool:** [ProcessorFuzz](https://github.com/bu-icsg/ProcessorFuzz) (Docker)
	- **Target RISC-V:**
		- [BOOM](https://github.com/riscv-boom/riscv-boom) (Chisel)
		- [BlackParrot](https://github.com/black-parrot/black-parrot) (Verilog)
		- [Rocket](https://github.com/chipsalliance/rocket-chip) (Verilog)
- Trippel et al., [Fuzzing Hardware Like Software](https://www.usenix.org/conference/usenixsecurity22/presentation/trippel), USENIX Security, 2022.
	- **Tool:** [googleintern/hw-fuzzing](https://github.com/googleinterns/hw-fuzzing) (Docker)
	- **Target RISC-V:**
		- [OpenTitan](https://github.com/googleinterns/hw-fuzzing/tree/master/hw/opentitan) (AES, HMAC, KMAC, Timer)
- Hur et al., [DifuzzRTL: Differential Fuzz Testing to Find CPU Bugs](https://lifeasageek.github.io/papers/jaewon-difuzzrtl.pdf), IEEE S&P, 2021.
	- **Tool:** [DifuzzRTL](https://github.com/compsec-snu/difuzz-rtl)
	- **Target RISC-V:**
		- [BOOM](https://github.com/riscv-boom/riscv-boom) (Chisel)
		- [mor1kx](https://github.com/openrisc/mor1kx) (Verilog)
		- [Rocket](https://github.com/chipsalliance/rocket-chip) (Verilog)
- Laeufer et al., [RFUZZ: Coverage-Directed Fuzz Testing of RTL on FPGAs](https://people.eecs.berkeley.edu/~ksen/papers/rfuzz.pdf), IEEE/ACM ICCAD, 2018.
	- **Tool:** [RFUZZ](https://github.com/ekiwi/rfuzz?tab=readme-ov-file) (VM)
	- **Target RISC-V:**
		- [Rocket](https://github.com/chipsalliance/rocket-chip) (Verilog)
		- [Sodor](https://github.com/ucb-bar/riscv-sodor) (Chisel)