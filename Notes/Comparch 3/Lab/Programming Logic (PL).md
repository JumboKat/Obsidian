Writing a normal program entails writing instructions for an existing machine that knows how to execute the program. You cannot change the machine, you can only change what you ask it to do.

A **Field-Programmable Gate Array (FPGA)** allows you to describe the machine itself, and the fabric then becomes that machine. "Field-programmable" means it is reprogrammable after it has left the factory, out in the 'field.' 

The fabric is a grid of components: 
- small lookup tables that can be configured to compute any small logic function
- flip-flops that can retain bits
- blocks of memory
- a switching network that decides how pieces connect to each other.

To program an FPGA, you write a giant configuration file known as the **bitstream** that sets each switch. 

There are two major caveats when dealing with PL:
- **Building the machine is slow; running it is fast**: working out how to place and wire your design onto the grid requires difficult optimization; generating a bitstream can take 10-20 mins just for one NAND gate. Once loaded, however, it responds in ns.
- **The machine does not exist until it is loaded**: only until xmutil loadapp is ran does the NAND gate exist.