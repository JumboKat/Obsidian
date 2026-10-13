ARM64 is an **Instruction Set Architecture (ISA)**. ARM64 defines what software relies on: instructions, registers, data types, how memory is accessed/ordered, how exceptions and interrupts work, and privilege levels and interfaces used by the OS. A compiler that targets ARM64 produces a binary that works on any chip that implements the ISA. This allows the same program to run on an Appl A19 Pro, Snapdragon, and an Arm Cortex core. Chip companies often license ISAs like ARM64 because it is already compatible with existing OS, applications, compilers, and development tools. It is much cheaper to license than to build all of this from scratch.

Everything else is determined by the chip designer; pipeline depth, FUs, branch predictor, OoO machinery, cache hierarchy, clock speeds, power management, and how CPU connects to memory, GPU, and NPU. Two chips can execute the same instructions with different speed and energy use. Companies design their own custom cores for these reasons:
- **Performance per watt**: tuning the microarchitecture for a desired target vs. a general-purpose design.
- **Integration with the whole SoC**: the CPU can be designed to integrate with other parts of the chip, like the GPU and NPU.
- **Roadmap control**: you don't have to wait on ARM's release schedule.

An Arm architecture license does not require Arm Cortex cores.