# R1 source register and evidence review

Back to the [primary note](../../001-sta-fundamentals.md).
Scope and outputs: [issue #23](https://github.com/kastor-st/kastor-sta/issues/23).
Revised: 2026-11-18.
Online access dates: W1/W2/W5–W8, 2026-11-17; W3/W4, 2026-11-06.

## Source register

The textbooks supply the conceptual foundation. Vendor and tool-maintainer
documentation corroborates practical interpretation. Vendor examples are not
evidence of universal tool behavior or Kastor STA capabilities. This is a targeted
conceptual literature review, not a systematic review or a tool benchmark.

| ID | Source and public locator | Material used and qualification |
| --- | --- | --- |
| B1 | J. Bhasker and Rakesh Chadha, *Static Timing Analysis for Nanometer Designs: A Practical Approach*, Springer, 2009, [DOI 10.1007/978-0-387-93820-2](https://doi.org/10.1007/978-0-387-93820-2) | §§1.2–1.6, 2.2, 2.6, 2.8–2.10, 3.4, 8.1–8.2. Established theory; historical implementation claims are not treated as current capability limits. |
| B2 | M. Morris Mano and Michael D. Ciletti, *Digital Design: With an Introduction to the Verilog HDL, VHDL, and SystemVerilog*, sixth edition, Global Edition, Pearson Education Limited, 2019. ISBN 978-1-292-23116-7. | §§4.1–4.3, pp. 164–165: combinational logic and functional analysis. §§5.1–5.4, pp. 262–272: sequential logic, clocks, latch storage, and edge-triggered flip-flops. |
| W1 | Synopsys, [What is Static Timing Analysis?](https://www.synopsys.com/glossary/what-is-static-timing-analysis.html), undated | Definition and path terminology. Introductory vendor explanation; “all paths” is qualified by the analysis setup. Marketing accuracy claims are not adopted. |
| W2 | Parallax Software, [OpenSTA documentation](https://opensta.readthedocs.io/en/latest/); pinned [README](https://github.com/parallaxsw/OpenSTA/blob/2b8d4dfb41ccd570687c207775ca8423a8da2d60/README.md) and [Debugging timing](https://github.com/parallaxsw/OpenSTA/blob/2b8d4dfb41ccd570687c207775ca8423a8da2d60/doc/Debugging.md) | Source commit `2b8d4dfb41ccd570687c207775ca8423a8da2d60`. README: input formats, clocks, timing engine. Debugging: “No paths found.” |
| W3 | AMD, *Vivado Design Suite User Guide: Logic Simulation*, UG900, 2026.1, 2026-07-08: [Running Timing Simulation Using Third-Party Tools](https://docs.amd.com/r/en-US/ug900-vivado-logic-simulation/Running-Timing-Simulation-Using-Third-Party-Tools) and [Post-Implementation Simulation](https://docs.amd.com/r/en-US/ug900-vivado-logic-simulation/Post-Implementation-Simulation) | Gate netlists and SDF; examples of issues timing simulation may expose. FPGA-specific documentation used for the analysis distinction, not an ASIC implementation recipe. |
| W4 | AMD/Xilinx, *Versal ACAP Hardware, IP, and Platform Development Methodology Guide*, UG1387, 2020.2, 2021-03-26: [Constraining Asynchronous Clock Groups and Clock Domain Crossings](https://docs.amd.com/r/2020.2-English/ug1387-acap-hardware-ip-platform-dev-methodology/Constraining-Asynchronous-Clock-Groups-and-Clock-Domain-Crossings) | Explicit warning that default timing analysis cannot prove asynchronous CDC functionality. Versioned source; detailed CDC constraints and protocols are outside R1. |
| W5 | Holger Vogt, Giles Atkinson, Dietmar Warning, Paolo Nenzi, [*Ngspice User’s Manual*, version 46](https://ngspice.sourceforge.io/docs/ngspice-46-manual.pdf), 2026-03-31 | §§1.1.1–1.1.3 and 1.2.3, especially printed p. 40 on transient analysis. Describes circuit simulation, not a measured accuracy comparison with STA. |
| W6 | MIT OpenCourseWare, *6.004 Computation Structures*, Spring 2009, [Lecture 5: Sequential Logic](https://ocw.mit.edu/courses/6-004-computation-structures-spring-2009/b06d402de9caa103a9229655280b1076_MIT6_004s09_lec05.pdf), 2009-02-19 | Slides 9–19, PDF pp. 4–6: latch storage/transparency, the dynamic discipline, and edge-triggered storage. Teaching examples explain behavior, not numerical requirements for arbitrary library cells. |
| W7 | AMD, *Vivado Design Suite User Guide: Design Flows Overview*, UG892, 2023.2, 2023-10-18, [Timing Simulation](https://docs.amd.com/r/2023.2-English/ug892-vivado-design-flows-overview/Timing-Simulation) | Gate-level simulation consumes SDF timing annotation. Used to corroborate that dynamic simulation also uses delay models, not to claim universal superiority in accuracy. |
| W8 | OpenLane, [Achieving Timing Closure](https://openlane2.readthedocs.io/en/stable/usage/timing_closure/index.html), guide adapted from Mohamed Shalan's teaching and implementation material | Timing reports ordered by slack, worst first, and annotated gate-level simulation. Used for these specific documentation observations, not as evidence of every generalization in the guide. |

### Book page locators

Page numbers refer to the printed pagination of the editions listed above.

| Book | Printed pages / sections | Supporting content |
| --- | --- | --- |
| B1 | pp. 2–12, §§1.2–1.6 | STA purpose, design flow, and limitations |
| B1 | p. 6, Fig. 1-2 | Digital design flow |
| B1 | pp. 20–21, §2.2 | Cell timing abstraction |
| B1 | pp. 34–39, §§2.8–2.10 | Timing paths, clock domains, and operating conditions |
| B1 | pp. 228–229, §8.1 | Setup definition and clock relationship |
| B1 | pp. 248–249, §8.2 | Hold definition and same-edge relationship |
| B2 | pp. 164–165, §§4.1–4.3 | Combinational versus sequential logic; verification of a stated logic function |
| B2 | pp. 262–264, §§5.1–5.2, Figs. 5.1–5.2 | State, synchronous/asynchronous sequential circuits, and clock coordination |
| B2 | pp. 267–269, §5.3, Fig. 5.6 and Practice Exercise 5.3 | D-latch transparency while enabled and storage while disabled |
| B2 | pp. 269–272, §5.4, Figs. 5.8–5.11 | Edge-triggered flip-flops and clock-to-output propagation |
| B2 | p. 272, §5.4 | Setup and hold requirements around the active clock edge |

## Question-to-evidence map

Answers appear in the primary note in the issue's original learning order.

| Question | Evidence and reasoning | Result |
| --- | --- | --- |
| 1: Definition and purpose | B1 §§1.2–1.3; W1 definition. Feasible frequency requires an explicit timing environment. | Answered |
| 2: Timing versus function | B1 §1.2; B2 §4.3 separates logical behavior from physical timing. B1 §§8.1–8.2 qualify the effect of slowing the clock. | Answered |
| 3: Design flow | B1 §§1.4–1.5, Fig. 1-2 explicitly includes clock-tree synthesis and analysis inside optimization. | Answered |
| 4: Combinational/sequential | B2 §§4.1–4.2, 5.2–5.4; W6 for latch transparency and storage. “After settling” distinguishes Boolean behavior from zero-delay behavior. | Answered |
| 5: Synchronous/asynchronous | B2 §5.2; B1 §1.4.3; W4 for asynchronous communication between clocked domains. | Answered |
| 6: Clocks/domains | B1 §§2.6, 2.9; W2 clock properties; W4 relationship/CDC distinction. | Answered |
| 7: Timing path | W1 path definitions; B1 §2.8 and §§8.1–8.2; original SVG names the precise pins. | Answered |
| 8: Setup/hold | B1 §§8.1–8.2; §3.4 qualifies signed library requirements and slew dependence. B2 §5.4, p. 272 defines setup, hold, and propagation delay; W6 explains data stability around storage events. | Answered |
| 9: Vector-free checking | B1 §§1.2, 1.6; W2 documents graph arrivals and constant propagation. No implementation algorithm is prescribed. | Answered |
| 10: Violations/criticality | B1 §2.8 and §8.1 timing reports (including negative slack), §8.2 for early timing; W8 documents worst-slack-first reports. Comparing slack instead of raw delay follows because endpoint requirements can differ. | Answered |
| 11: Dynamic timing/GLS | B1 §§1.2–1.3; W3/W7 distinguish functional simulation and timing simulation with SDF. B2 §4.3 motivates checking observed behavior against the intended function. | Answered |
| 12: Comparison | B1 §§1.3, 1.6; W3/W4; table below separates coverage from model fidelity. | Answered |
| 13: Circuit simulation | W5 device/transient descriptions; B1 §§1.6, 2.2. Suggested use cases follow from questions that a timing abstraction omits. | Answered |
| 14: Validity/non-guarantees | B1 §§1.6, 2.10; W2 “No paths found”; W4 CDC qualification. Review below states necessary assumptions. | Answered |

## Diagram interpretation

The [register-path SVG](register-path.svg) is an illustrative schematic, not an
executable netlist, measured circuit, or copied textbook figure. Named pins and
connections are the authoritative topology for this conceptual example:

| Connection or arc | Role |
| --- | --- |
| `CLK -> U_L/CK` | Launch clock branch |
| `CLK -> U_C/CK` | Capture clock branch |
| `U_L/CK -> U_L/Q` | Launch clock-to-Q timing arc inside the flip-flop |
| `U_L/Q -> U_INV/A` | First data net |
| `U_INV/A -> U_INV/Y` | Inverter timing arc |
| `U_INV/Y -> U_BUF/A` | Second data net |
| `U_BUF/A -> U_BUF/Y` | Buffer timing arc |
| `U_BUF/Y -> U_C/D` | Final data net, ending at the capture data pin |

`U_L` and `U_C` are one-bit rising-edge registers. `U_INV` and `U_BUF` are
combinational cells. The inverter changes transition polarity; the buffer
preserves it. No delay values, loading, PVT, or sign-off margins are specified,
so no numerical slack follows from the drawing.

For setup in this example, data launched on one clock edge must meet the next
capture edge's setup requirement. For hold, newly launched data must not disturb
the value being captured on the corresponding same edge too soon. Clock arrival
times at `U_L/CK` and `U_C/CK` need not be equal merely because they share `CLK`.
The simplified picture does not imply zero clock skew. [B1 §§8.1–8.2]

The CK/Q distinction reconciles common terminology: a data route is often drawn
Q-to-D, while the launch event and its clock-to-Q delay must still be included.
The note adopts W1's clock-pin startpoint convention explicitly. Tool reports
may print the launching cell name instead of a CK pin; this is a presentation
distinction, not a missing launch delay.

## Comparison of analysis approaches

This table is research synthesis of B1 §§1.2–1.6, W3, and W5. “Coverage” and
“electrical detail” are separate dimensions; more electrical detail does not
automatically mean more scenarios were verified.

| Approach | Inputs and abstraction | Strength | Main limitation / complementary role |
| --- | --- | --- | --- |
| STA | Connectivity, timing models, interconnect representation, constraints; propagation of timing bounds | Broad coverage of enabled, constrained timing checks without enumerating input sequences | Can be pessimistic about infeasible paths or optimistic through missing constraints/models; cannot establish intended functionality |
| Gate-level timing simulation | Gate netlist, simulation cell models, delays (often SDF), testbench and initial state | Shows temporal logical behavior of exercised sequences; useful for reset/startup and integration debugging | Unexercised behavior remains unchecked; delay/event and X semantics are model-dependent, not analog waveforms |
| Circuit-level transient simulation | Devices, electrical interconnect, device models, supplies, initial conditions, stimulus waveforms | Examines voltage/current trajectories and waveform effects beyond the gate abstraction | Higher computational cost and selected-scenario coverage; depends on device models, numerical settings, and initial conditions |

Functional RTL simulation and formal property/equivalence checking address
functional questions complementary to this table. Timing-aware gate simulation
may reveal modeled glitches or X propagation but is not proof of analog
metastability resolution or safe CDC protocols. SPICE can investigate electrical
behavior in a selected circuit; a successful transient run is not a proof over
all metastability events, operating conditions, or input histories.

## Validity and limitations

These are conditions on interpreting a result, not new Kastor STA requirements:

1. **Representative circuit:** the analyzed netlist and cell mappings correspond
   to the implementation and mode being assessed. Black boxes, missing arcs,
   disabled arcs, and unsupported structures require an explicit account.
2. **Representative electrical models:** cell characterization, input slew,
   output loading, interconnect, and units must be consistent. Extracted
   post-route parasitics improve physical information but do not eliminate
   modeling uncertainty. [B1 §§1.4–1.5, 2.2, 2.10]
3. **Complete timing environment:** relevant clocks, their relationships,
   uncertainty and distribution, input arrival assumptions, and output
   requirements must be specified. A path with no meaningful requirement cannot
   establish timing correctness by having no reported failure. W2 explicitly
   describes constrained versus unconstrained path reporting.
4. **Justified exceptions:** false paths, multicycle paths, and operating-mode
   assumptions must agree with intended behavior. Removing a real failing path
   through an exception does not fix the circuit. [B1 §1.6]
5. **Relevant scenarios and checks:** a pass at one process/voltage/temperature
   point is not a pass at all points. Both late and early timing matter; include
   applicable pulse-width, recovery/removal, gating, and I/O checks as supported
   and required. Variation and signal-integrity effects are conditional on the
   models and analysis enabled. [B1 §§2.10, 8.1–8.2; W1]
6. **Independent functional and interface verification:** STA assumes the clock
   and operating environment described; it does not prove that reset sequences,
   clock configuration, synchronizers, protocols, or analog blocks behave as
   intended. [B1 §1.6; W4]

Thus a defensible conclusion is “the analyzed checks meet their constraints
under the stated models and scenarios,” not an unconditional hardware guarantee.
The absence of a reported violating path must be distinguished from evidence
that required paths were actually constrained and checked.

## Conceptual distinctions retained in the note

These distinctions explain the qualifications in the fourteen answers. They
are supported by the sources mapped above and do not define additional
implementation requirements for Kastor STA.

| Common simplification | Technical qualification |
| --- | --- |
| STA answers maximum-frequency and constraint-satisfaction questions | Clock, interface, and mode assumptions must be explicit. Frequency is not the only constraint: hold and other checks still matter. |
| STA is fast because it does not need a real operating circuit | Pre-silicon simulation also needs no physical chip. STA's distinguishing advantage is avoiding input-sequence simulation while checking modeled timing relationships broadly. |
| Accuracy depends on operating conditions and values | Process, supply voltage, temperature, loading, slew, interconnect, and switching conditions affect timing. “Static” does not mean invariant delay; abstraction and scenario coverage determine applicability. |
| Running faster or slower than the circuit's timing can cause failure | For conventional fully static, edge-triggered logic with unchanged edge relationships, slowing the clock generally eases setup; ordinary same-edge hold is period-independent. A lower-frequency limit needs a specific device, dynamic-storage, interface, or clock-generation requirement. |
| Timing verification is mandatory for functional correctness | Physical implementation correctness and logical functional correctness are separate concerns. A circuit may pass one category and fail the other; STA alone proves neither the intended function nor all physical behavior. |
| STA is used within and after logical and physical stages | Clock-tree synthesis is also part of the flow. Early stage timing checks guide implementation; final timing sign-off uses appropriate final models and constraints. Passing a stage check is not final chip sign-off. |
| Post-route timing is most realistic | Final extraction and clock implementation supply better physical information, but accuracy and coverage still depend on models, scenarios, and constraints. |
| A combinational block has one clock period, excluding surrounding logic | Its budget follows from the full path and constraints. Same-clock, single-cycle setup is one case; I/O timing and multicycle relationships can differ. |
| Latches follow data immediately and are necessarily asynchronous | A D latch follows data only while enabled, then retains it. Latches can form part of a synchronous design; coordination of state updates defines the distinction. |
| A clock domain crossing is always asynchronous | Clock relationships matter. Distinct domains can use related clocks, while equal nominal frequencies alone do not establish a dependable phase relationship. |
| Setup and hold exist because clock edges are not instantaneous | Storage requires data stability around capture. Clock/data slew affects characterized requirements, but finite clock rise time alone is not their cause. |
| Maximum path delays are enough to prove timing | Setup needs late timing and hold needs early timing, including clock effects. Results remain conditional on complete constraints, valid models, and scenario coverage. |
| A critical path has a high probability of failure | Ordinary deterministic STA ranks timing margin, not failure probability. A critical path can pass; report-count limits and optimization thresholds do not redefine slack. |
| Dynamic timing simulation avoids models and is inherently more accurate | It uses delay and simulation models for exercised sequences. Distinguish model fidelity from scenario coverage, and compare responses with expected results to check functionality. |
| SPICE is a final guarantee after digital timing passes | Circuit simulation addresses electrical questions beyond the abstraction, such as a custom cell's loaded output waveform. It remains model- and stimulus-dependent. |

## Source qualifications and disagreements

- B1 and W1 use exhaustive or all-path language. This note restricts that claim
  to the timing model and configured checks. W2's documented absence of reported
  paths for unconstrained cases is concrete evidence that silence is not a
  coverage guarantee.
- B1 §2.9 uses “independent” partly to describe domains with no connecting data
  paths. R1 distinguishes structural connectivity from clock phase relationship;
  the existence of a crossing does not establish synchronous clocks.
- B1 §2.9 describes synchronized crossings using “false path” language. A
  physical signal still traverses such a crossing; excluding a synchronous
  check does not establish a safe synchronizer, correct multi-bit protocol, or
  unlimited freedom in physical implementation. W4 explicitly requires CDC
  review. No blanket false-path command is recommended here.
- B1's simple setup/hold diagrams use positive intervals. Its §3.4 discusses
  signed constraints; this note does not elevate the positive-interval picture
  to a universal rule for every library cell.
- W3 calls post-implementation delays “actual.” Here that means derived from
  the implemented design, not a measurement of every manufactured chip.
- B1 is from 2009. Statements about the then-current limits of noise analysis
  or individual tools are not adopted as limits of all present-day STA. R1
  makes no comparative tool capability or accuracy claim.

## Review outcome

All fourteen questions have an answer and source mapping, and the required
diagram identifies both registers, combinational cells, clock connections, and
startpoint/endpoint conventions. No agreed question remains unresolved. The
investigation is conceptual; no numerical experiment is required.
Detailed calculations and format/constraint choices remain
in the already-scoped follow-ups linked by the primary note. No immediate
project decision is proposed.
