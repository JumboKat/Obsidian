**Dynamic Random Access Memory (DRAM)** serves as the main memory component in computing devices. Each bit is represented by one transistor and a capacitor. A charge represents 1, while no charge represents 0. This allows DRAM to be dense and cheap as only two components are needed to store information.
![[Pasted image 20260924105620.png]]

Capacitors leak their charge over time, so the rows are refreshed (read and rewrite) periodically (every ~64 ms) while it is on to retain its information. This refresh overhead is partly why DRAM is so slow and power-hungry. Additionally, reads are destructive; the act of sensing whether a capacitor is charged or not actually drains it. This means that after every read (including every memory access), the row has to be rewritten to be restored.

SRAM, which uses 6 transistors but no capacitor, does not leak; it does not suffer from the refreshing overhead and is much faster. Thus, while it is more expensive per bit, it is used for small, fast caches.

DRAM latency has barely improved throughout its history. Thus, improvements have instead focused on increasing bandwidth and parallelism.
### DRAM Organization
![[Pasted image 20260924105822.png]]
DRAM is hierarchical and allows for parallelism on multiple levels.
- The entire memory communicates via a single memory channel (64-bit wide data bus).
- A **rank** corresponds to one side of a **DIMM** (*Dual In-Line Memory Module*) stick and contains a set of 8 chips.
- Together, the 8 chips, each contributing 8 bits, form a 64-bit word (or 8 bytes). All 8 chips receive the same command and act in unison.
- Each chip has a grid of 32 banks, each of which is a 2D array of cells. Banks are independent, so one can be precharging/preparing while another is actively streaming data. 
- The **row/column** are the coordinates in the bank's array. Activating a row copies its entire contents into the row buffer. When the controller requests a specific memory address, it corresponds to the same bank across all 8 chips simultaneously. 

A row-buffer hit (accessing data in a row that's already in the row buffer) is fast, since the data has already been read. A row miss is slow, because it now must precharge then activate the new row before it can access it. This means that spatial locality is important.
### DDR
**DDR** stands for *Double Data Rate*, meaning data is transferred on both the rising and falling edges of the clock, so the transfer rate (mega-transfers/second or MT/s) is twice the I/O clock. Newer generations of DDR have widened the **prefetch** (bits fetched per column access) to feed faster buses.
#### Example: DRAM Bandwidth
For a dual-channel DDR5-6400 laptop, the peak bandwidth is given by the transfer rate times the width of the memory channel times the number of channels:
$$BW_{peak}=6400\frac{MT}{s}\times8B\times2$$
