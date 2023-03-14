###### tags: `System Software` `RISC` `PowerPC`

<style>
    font {
        color: red;
        font-weight: bold;
    }
</style>

# PowerPC Architecture
- IBM first introduced the POWER architecture early in 1990 with the RS/6000.
- POWER is an acronym for Performance Optimization With Enhanced RISC.
- It was soon realized that this architecture could form the basis for a new family of powerful and low-cost microprocessors.
- In October 1991, IBM, Apple, and Motorola formed an alliance to develop and market such microprocessors, which were named PowerPC.

##  Memory
- Two consecutive bytes: halfword
- Four bytes: word
- Eight bytes: doubleword
- Sixteen bytes: quadword
- Virtual address space: $2^{64}$ bytes
- The address space is divided into fixed-length segments: 256 megabytes long.
- Each segment is divided into pages: 4096 bytes long.

##  Registers
- 32 general-purpose registers: 64 bits long.
- 32 floating-point registers for FPU: 64 bits
- A 32-bit condition register
- Link Register (LR) and Count Register (CR) used by some branch instructions

##  Data formats
- Integers are stored as 8-, 16-, 32-, or 64-bit binary numbers.
- Big-endian byte ordering by default.
- It is possible to select little-endian byte ordering by setting a bit in a control register.
- There are two different floating-point data formats.
- Single-precision format is 32 bits long (23+8+1)
- Double-precision format is 64 bits long (52+11+1)

##  Instruction format
- There are seven basic instruction formats, some of which have subforms.
- All of these formats are 32 bits long
- Instructions must be aligned beginning at a word boundary
- The first 6 bits of the instruction word always specify the opcode; some instruction formats also have an additional “extended code” field.
- The fixed instruction length in the PowerPC architecture is typical of RISC systems, making instruction decoding faster and simpler than on CISC systems.

##  Addressing modes
- Immediate mode
- Register direct mode
- The only instructions that address memory are load and store operations, and branch instructions.
- Load and store operations use one of the following three addressing modes.
- Register indirect mode: TA=(register)
- Register indirect with index: TA=(register1)+(register2)
- Register indirect with immediate index: TA=(register)+displacement
- The register numbers and displacement are encoded as part of the instruction.
- Branch instructions use one of the following three addressing modes.
- Absolute: TA = actual address
- Relative: TA = current instruction address + displacement
- Link Register: TA = (LR)
- Count Register: TA = (CR)
##  Instruction set
- Approximately 200 machine instructions
- Floating-point “multiply and add” instructions
- Using more powerful instructions, so fewer instructions are required to perform a task vs. keeping instructions simple so they can be executed as fast as possible.
- Instruction execution on a PowerPC system is pipelined. However, the pipelining is more sophisticated than on the original SPARC systems, with branch prediction used to speed execution. As a result, the delayed branch technique we described for SPARC is not used on PowerPC and most other modern architectures.

## Input and Output
- Segments in the virtual address space are mapped onto an external address space (typically an I/O bus)