###### tags: `System Software` `RISC` `UltraSPARC`

<style>
    font {
        color: red;
        font-weight: bold;
    }
</style>

# UltraSPARC (Scalable Processor ARChitecture) Architecture

##  Memory
- <font>Halfword</font>: two consecutive bytes
- <font>Word</font>: 4 bytes
- <font>Doubleword</font>: 8 bytes
- Data are stored in memory with alignment
- Virtual address space of $2^{64}$ bytes
- The address space is divided into pages. Multiple page size are supported.

##  Registers
- A large **register file** that usually contains more than 100 general-purpose register.
- However, any procedure can access only 32 registers.
- The **first eight** of these registers are **global** --- accessed by all procedures on the system.
- **Register *r0* always contains the value zero.**
- The other 24 registers available to a procedure can be visualized as a **window** through which part of the register file can be seen.
- These windows overlap, so some registers in the register file are shared between procedures. This facilitates the passing of parameters.
- If a set of concurrently running procedures needs more windows than are physically available, a "window overflow" interrupt occurs. The operating system must then save the contents of some registes in the file (and restore them later) to provide the additional windows that are needed.

##  Data Format
- **Integers**, **floating-point** values, and **characters**.
- Support both big-endian and little-endian byte orderings.
- Three different floating-point data formats.

##  Instruction Formats
- There are three basic instruction formats. All of these formats are **32 bits** long. **The first 2 bits of the instruction word identify which format is being used**.
- **Format 1** is used for the **Call instruction**.
- **Format 2** is used for **branch instruction** (and one special instruction that enters a value into a register)
- **Format 3** is used by the **remaining instructions**, including **register loads** and **stores**, and **three-operand arithmetic operations**.

##  Addressing Modes 
- **Immediate mode**
- **Register direct mode**
- Operands in memory are addressd using one of the following three modes:
    - ***PC-relative*: TA = (PC) + displacement {30 bits, signed}**
    - ***Register indirect with displacement*: TA = (register) + displacement {13 bits, signed}**
    - ***Register indirect indexed*: TA = (register1) + (register2)**
- PC-relative mode is used only for branch instructions.

##  Instruction Set
- The basic SPARC architechture has fewer than 100 machine instructions.
- Instruction execution on a **SPARC** system is <font>pipeline</font>: fetching_and_decoding, execution
- However, an ordinary branch instruction might cause the process to "stall". The instruction following the branch would have to be dicarded without being executed.
- To make the pipeline work more efficiently, SPARC branch instructions (including subroutine calls) are <font>delayed branch</font>. This means that the instruction immediately following the branch instruction is actually executed before the branch is taken.