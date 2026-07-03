# Reliable CMOS PUF Cell for IoT Hardware Security

A Digital Integrated Circuits project implementing a **Schmitt-trigger-assisted SRAM Physical Unclonable Function (PUF)** for low-cost IoT hardware authentication. The design uses natural CMOS transistor mismatch as a hardware fingerprint source, then improves response reliability using Schmitt-trigger-assisted readout and unstable-bit detection.

This repository contains the Electric VLSI schematic/layout libraries, project report, presentation, and reference material used for the final project.

---

## Project Overview

Physically Unclonable Functions (PUFs) generate chip-unique digital responses from unavoidable manufacturing variations. Instead of storing a secret key in non-volatile memory, a PUF regenerates the key from the physical characteristics of the circuit, making it useful for secure identification and authentication in embedded and IoT devices.

This project focuses on an **SRAM-based CMOS PUF**. A conventional 6T SRAM cell naturally powers up to either `0` or `1` depending on small transistor mismatches. However, some cells may become unstable due to noise, voltage variation, temperature variation, or weak mismatch. To improve reliability, this design adds:

- a **Schmitt trigger-assisted readout circuit** for better noise immunity,
- a **4x4 SRAM PUF array** for generating a 16-bit response,
- an **output latch** for storing generated responses,
- an **unstable-bit detection circuit** for identifying unreliable bits,
- a **masking/stabilization stage** for preserving stable response bits.

---

## Main Features

- CMOS inverter schematic and layout implementation
- Conventional 6T SRAM cell schematic and layout
- Schmitt trigger readout circuit schematic and layout
- One-bit SRAM PUF readout unit
- 4x4 SRAM PUF array design
- Latch-based output storage
- Unstable-bit detection concept
- Documentation through IEEE-style report and project presentation
- Reference-paper-based design inspired by ASCH-PUF reliability techniques

---

## System Architecture

The design is built hierarchically from basic CMOS blocks to the final PUF array:

```text
CMOS Inverter
     ↓
6T SRAM Cell
     ↓
Schmitt Trigger-Assisted Readout
     ↓
One-Bit SRAM PUF Readout Unit
     ↓
4x4 SRAM PUF Array
     ↓
Output Latch + Unstable-Bit Detection
     ↓
Masked / Stabilized PUF Response
```

### Block Descriptions

| Block | Purpose |
|---|---|
| CMOS Inverter | Basic CMOS logic building block used inside SRAM and support circuits. |
| 6T SRAM Cell | Entropy source that generates a power-up response based on transistor mismatch. |
| Schmitt Trigger | Improves readout stability by using hysteresis and different switching thresholds. |
| One-Bit Readout Unit | Combines the SRAM PUF cell and Schmitt trigger readout into a single response unit. |
| 4x4 PUF Array | Generates a 16-bit PUF response using sixteen SRAM PUF cells. |
| Output Latch | Stores the generated PUF response after readout. |
| Unstable-Bit Detector | Flags bits that may change across repeated evaluations. |
| Masking Stage | Removes unreliable bits to improve repeatability of the final PUF response. |

---

## Repository Structure

```text
Project/
├── CMOScells.jelib                         # Standard/reference CMOS cells used by Electric
├── Gates.jelib                             # Basic logic gates: inverter, NAND, NOR
├── Inverter1.jelib                         # Custom CMOS inverter design
├── SRAM_CELL.jelib                         # 6T SRAM cell schematic/layout library
├── Schmitt.jelib                           # Schmitt trigger schematic/layout library
├── RisingFlipFlop_nandpLatches.jelib       # Latch and rising-edge flip-flop library
├── PUF_Project.jelib                       # One-bit PUF readout design
├── PUF_4x4_Array.jelib                     # 4x4 SRAM PUF array design
├── ProjectDescription.pdf                  # Original project handout/specification
├── ReferencePaper.pdf                      # Main ASCH-PUF reference paper
├── Reliable_CMOS_PUF_Cell_paper.pdf        # IEEE-style project report
└── Reliable-CMOS-PUF-Cell.pptx             # Final project presentation
```

---

## Tools Used

- **Electric VLSI 9.07**
  - schematic design
  - layout design
  - hierarchy integration
  - DRC/LVS checking workflow
- **MOCMOS technology** inside Electric VLSI
- **SPICE / LTspice-compatible simulation flow**
  - transient simulation
  - voltage transfer characteristic simulation
  - power estimation
- **IEEE two-column report format** for documentation

> Note: SPICE model files such as the C5 CMOS model library may need to be installed locally and referenced with the correct `.include` path before running simulations.

---

## How to Open the Design in Electric VLSI

1. Clone the repository:

   ```bash
   git clone https://github.com/<your-username>/<your-repository>.git
   cd <your-repository>/Project
   ```

2. Open **Electric VLSI**.

3. Load the required `.jelib` libraries from the `Project/` directory.

4. Start with the main project libraries:

   - `PUF_4x4_Array.jelib`
   - `PUF_Project.jelib`
   - `SRAM_CELL.jelib`
   - `Schmitt.jelib`
   - `RisingFlipFlop_nandpLatches.jelib`
   - `Gates.jelib`
   - `Inverter1.jelib`

5. Open the schematic or layout views using Electric's cell explorer. Important cells include:

   - `Schmitt{sch}` / `Schmitt{lay}`
   - `SRAM_CELL{sch}` / `SRAM_CELL{lay}`
   - `Readout_1bit{sch}` / `Readout_1bit{lay}`
   - `PUF_4x4_Array{sch}` / `PUF_4x4_Array{lay}`
   - `risingFF{sch}` / `risingFF{lay}`

6. Run verification checks as needed:

   - DRC for layout-rule correctness
   - LVS/NCC for schematic-layout matching
   - SPICE export for circuit simulation

---

## Simulation Workflow

The general verification process used in this project is:

1. Design the block schematic in Electric VLSI.
2. Create the corresponding physical layout.
3. Run layout DRC to check design-rule correctness.
4. Compare schematic and layout using LVS/NCC.
5. Export the circuit netlist to SPICE.
6. Simulate using LTspice or a compatible SPICE simulator.
7. Verify the behavior of each block individually before integrating the full PUF system.

Recommended simulations:

- CMOS inverter VTC simulation
- SRAM write/hold transient simulation
- Schmitt trigger transient response
- one-bit PUF readout simulation
- 4x4 PUF array transient simulation
- unstable-bit detector simulation
- average power estimation

---

## Reported Results

The final report summarizes the following implementation results:

| Parameter | Reported Value |
|---|---:|
| SRAM cell area | 7,812 |
| Schmitt-assisted readout area | 114,758 |
| 4x4 SRAM PUF array area | 523,349 |
| SRAM average power | 0.502 mW |
| Schmitt-assisted readout average power | 1.683 uW |
| 4x4 array average power | 17.56 uW |

The results show that the Schmitt-assisted readout improves response stability and noise immunity while maintaining low power consumption. The main trade-off is the additional area introduced by the stabilization and readout circuitry.

---

## Reference Paper

This project was inspired by:

> Yan He, Dai Li, Zhanghao Yu, and Kaiyuan Yang, "ASCH-PUF: A Zero Bit Error Rate CMOS Physically Unclonable Function With Dual-Mode Low-Cost Stabilization," IEEE Journal of Solid-State Circuits, 2023.

The project does not directly copy the ASCH-PUF circuit. Instead, it uses the paper as technical inspiration for reliability improvement concepts such as unstable-bit detection, masking, and stabilization.

---

## Team

- Lujain Alawneh
- Kareem Kamel
- Ahmad Ouri
- Yanal Omar

Department of Electrical and Computer Engineering  
Birzeit University  
ENCS3330 - Digital Integrated Circuits

---

## Notes and Troubleshooting

- If Electric reports a missing referenced library, make sure all `.jelib` files are placed in the same project directory before opening the main design.
- Some libraries may reference local file names from the original development machine. If a dependency warning appears, reload the library manually from the `Project/` folder.
- If SPICE simulation fails because of missing transistor models, update the `.include` statement to point to the correct local CMOS model file.
- If the exported SPICE netlist contains unsupported Electric-generated names, rename problematic node/model references or clean the netlist before simulation.
- Before using this repository as a final submission, rerun DRC, LVS/NCC, and simulation on the target machine to confirm that all paths and models are configured correctly.

---

## License

This repository is intended for academic use as part of a Digital Integrated Circuits course project. If reused, please cite the team and the reference paper listed above.

---

## Future Work

Future improvements may include evaluating the proposed SRAM PUF under different operating conditions such as temperature and supply voltage variations, as well as implementing larger PUF arrays to further study scalability and reliability.
