###### tags: `System Software` `CSIC` `VAX`

<style>
    font {
        color: red;
        font-weight: bold;
    }
</style>

# VAX (Virtual Address eXtension) Architecture

## Memory
- 8-bit bytes
- Byte Addresses
- <font>Word</font>: 2 bytes
- <font>Long Word</font>: 4 bytes
- <font>Quadword</font>: 8 bytes
- <font>Octaword</font>: 16 bytes

Some operations are more efficient when operands are **aligned in a particular way**
- For example, a **longword** operand that **begins at a byte address** that is a **multiple of 4**.

<font>Virtual address space</font>: $2^{32}$ bytes
- One **half** of the VAX virtual address space is called <font>system space</font>, which **contains the operating system**, and is **shared by all programs**.
- The other half of the address space is called <font>process space</font>, and is defined separately for each program.

## Registers
:::danger
- 16 **general-purpose registers**: *R0*~*R15*, **32 bits** in length.
- ***R15*: program counter**
- ***R14*: stack pointer**
- ***R13*: frame pointer**
- ***R12*: argument pointer**
- **Processor status longword (*PSL*)**, which contains state variables and flags associated with a process.
:::

## Data Formats
- **2’s complement** representation is used for negative values. 
- Characters are stored using their **8-bit ASCII codes**.
- There are four different **floating-point** data formats, ranging in length **from 4 to 16 bytes**.
- **Packed decimal format:** each byte represents two decimal digits.
- **Numeric format:** one digit per byte.
- Support **queues** and **variable-length bit strings**.

## Instruction Formats
- A **variable-length instruction format**.
- Each instruction consists of **an operation code** (1 or 2 bytes) followed by **up to six operand specifiers**.

## Addressing Modes
- *Immediate* mode
- *Register* mode
- ***Register deferred*** mode
- *Autoincrement* and *autodecrement*  modes (register content)
- Several base relative addressing modes
- May also include an index register

## Instruction Set
:::info
- Instruction mnemonics (助記法) are formed by combining the following elements:
    1. A **prefix** that specifies the **type of operation**
    2. A **suffix** that specifies the **data type of the operands**
    3. A **modifier** (on some instructions) that gives the **number of operands** involved.
- Examples:
    - `ADDW2`: **ADD** **W**ord, **2** operands
    - `MULL3`
    - `CVTWL`
:::
There are number of operations that are much more complex than the machine instructions found on most computers. 

These operations are hardware realizations of frequently occurring **sequences of code**. They are **implemented as single instructions** for efficiency and speed.
- Load and store multiple registers
- Manipulate queues and variable-length bit fields.
- Calling and returning from procedures: a single instruction saves a designated set of registers, passes a list of arguments to the procedure, **maintains the stack, frame, and argument pointers**, and sets a mask to enable error traps for arithmetic operations. 

## Input/Output

- Each I/O device controller has a set of **control/status and data registers**, which are **assigned locations in the physical address space**. 
- The portion of the address space into which the device controller registers are mapped is called <font>I/O space</font>.
- No special instructions are required to access registers in I/O space.
- An I/O device **driver** issues commands to the device controller by **storing values into the appropriate registers**.
- Software routines may read these registers to obtain status information.