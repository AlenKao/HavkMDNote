###### tags: `System Software` `RISC` `Cray T3E`

# Cray T3E Architecture
* Supercomputer
* A massively parallel processing (MPP) system
* For use on technical applications in scientific computing.
* Containing a large number of processing elements (PE), arranged in a three-dimensional network as illustrated in Fig. 1.8.
    * The interconnection network is circular in each dimension.
* Each PE consists of a DEC Alpha EV5 RISC microprocessor, local memory, and performance-accelerating control logic developed by Cray.
* A T3E system may contain from 16 to 2048 processing elements.

##  Memory
* Each PE in the T3E has its own local memory with a capacity of from 64 megabytes to 2 gigabytes.
* The local memory within each PE is part of a physically distributed, logically shared memory system.
* Two consecutive bytes: word
* Four bytes: long word
* Eight bytes: quadword
* 64-bit virtual address

##  Registers
* 32 general-purpose registers: 64 bits long
* 32 floating-point registers: 64 bits long
* 64-bit program counter

##  Data formats
* Integers are stored as longwords or quadwords
* There are two different types of floating-point data formats. One group of three formats is included for compatibility with the VAX architecture. The other group consists of four IEEE standard formats, which are compatible with those used on most modern systems.
* There are no byte load or store operations. Only longwords and quadwords can be transferred between a register and memory. As a consequence, characters that are to be manipulated separately are usually stored one per longword.

##  Instruction formats
* There are five basic instruction formats, some of which have subforms.
* All of these formats are 32 bits long.
* The first 6 bits of the instruction word always specify the opcode; some instruction formats also have an additional “function” field.

##  Addressing modes
* Immediate mode
* Register direct mode
* PC-relative: TA = (PC) + displacement (for conditional and unconditional branches)
* Register indirect with displacement: TA = (register) + displacement (for load and store operations and for subroutine jumps)

##  Instruction Set
* Approximately 130 machine instructions
* The instruction set is designed so that an implementation of the architecture can be as fast as possible. For example, there are no byte or word load and store instructions. This means that the memory access interface does not need to include shift-and-mask operations.

##  Input and Output
* Performing I/O through multiple ports into one or more I/O channels.
* These channels are integrated into the network that interconnects the processing nodes.
* A system may be configured with up to one I/O channel for every eight PEs. 
* All channels are accessible and controllable from all PEs.
