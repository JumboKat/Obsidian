![[Pasted image 20260927115002.png]]
This **MPSoC** boasts two processor clusters: an **Application Processing Unit (APU)** with four A53s and a **Real-Time Processing Unit (RPU)** with two R5Fs and a TCM. The APU runs high-level OS while the RPU runs real-time/safety code. They are peers on the same interconnect, not master and slave. Through the interconnect, they share **On-Chip Memory (OCM)**, DDR, peripherals, and programming logic. They are also connected via an IPI doorbell.
### Multiprocessor Communication
Traditionally, there are two approaches to facilitating communication between processors:
- **Shared memory**: cores read/write to a common address space. Communication is done implicitly; one writes, the other reads. Synchronization and coherence protocols are needed.
- **Message passing**: each core has its own private memory. They transfer data to each other directly via messages. Synchronization is built into the sent/receival of messages.

![[Pasted image 20260927121151.png]]
The MPSoC, uses a hybrid solution: a shared memory with an **inter-processor interrupt (IPI)** doorbell. The doorbell does not send any data; instead it sends a interrupt letting another processor know that there is data in the shared memory for it to retrieve.
#### OpenAMP
OpenAMP is a software; a portable framework and set of conventions that runs on both cores. It makes use of the shared memory + doorbell to implement message passing. The stack consists of the following layers:
- Shared memory + doorbell IRQ: the physical foundation.
- virtio / vrings: OpenAMP uses **virtio** to connect two physical cores; virtio is typically used for connecting virtual devices inside VMs. **vrings** are ring buffers in shared memory. One processor writes in the ring buffer, while the other reads. Once the writing processor reaches the end of the ring, it cycles through the ring again, writing over the data it previously wrote.
- **Remote Proessor Messaging (RPMsg)**: adds named channels with endpoints; several independent logical conversations can share the same link, with dedicated channels for different data, to avoid them getting mixed up.
- **Application/callback**: The application that sends and receives named messages and reacts to them via callbacks.
![[Pasted image 20260927121359.png|351]]