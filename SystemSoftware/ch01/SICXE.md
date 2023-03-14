###### tags: `System Software` `SIC` `SIC/XE`
<style> 
    font {
        color: red;
        font-weight: bold;
    }
</style>

# SIC/XE Machine Architecture

## [Memory](https://www.youtube.com/watch?v=SlPhMPnQ58k) :+1: 

- **1 megabyte** ($2^{20}$ bytes)
- Larger memory $\Rightarrow$ a change in **instruction formats** and **addressing** (需要更多的 bit 來表示記憶體位置)

## <font>Registers</font>
:::danger
比[標準版本](https://hackmd.io/@ChienI/sic#Registers)**增加**以下四個暫存器:
- **B**: **Base register** for addressing，基底暫存器，**用在定址**
- **S**: General working register
- **T**: General working register
- **F**: **Floating-point accumulator**，浮點累加器，用於浮點運算 (**48 bits**)
:::

#### General Working/Purpose Register
- 沒有用在任何特殊目的的暫存器
- 可以儲存任意資料
- 用途由程式設計者自行決定

## Data Formats

整數、字元的資料格式和和標準 SIC 相同

**In addition**, there is a **48-bit floating point** data type:
![](https://i.imgur.com/Qn9UxiC.png)


浮點數格式使用 [**IEEE 754**](https://zh.wikipedia.org/zh-tw/IEEE_754) 標準

- **fraction** ($F$)
    - the fraction is interpreted as a value between 0 and 1
    - the high-order bit (最左位) of the fraction must be 1
- **exponent**: ($e$)
    - the exponent is interpreted as an unsigned binary number between 0 and 2047
- **s**: **the sign of the value**
    - **0: postive**
    - **1: negative**
- The value of the floating-point number:
    $$
    F \times 2^{(e-1024)}
    $$
- A value of 0 is represented by all 0

:::info
- **Bit string to value**
    ```
    | s |   exponent    |                  fraction                    |
    +---+---------------+----------------------------------------------+
    | 0 | 100 0000 0010 | 0101 0000 0000 0000 0000 0000 0000 0000 0000 |
    ```
    - $s$: postive
    - $e$: $1026$
    - $F$: $(1.0101\ 0000...)_2 = 2^0 + 2^{-2} + 2^{-4} = 1.3125$
    - The value of the floating point number
        $$
        \begin{split}
            &1.3125 \times 2^{(1026-1024)} \\
            =&1.3125 \times 2^2 \\
            =&5.52
        \end{split}
        $$
:::

## Instruction Formats
 
### **Format 1** (1 byte)
- 不用指定運算元的指令，如特定暫存器 + 1
![](https://i.imgur.com/Qfl6718.png)

### **Format 2** (2 byte)
- 指定暫存器 address 進行運算
![](https://i.imgur.com/oeO2joD.png)

### **Format 3** (3 byte)
- **大多數指令為此格式**
- **Relative addressing mode 一定是 format 3**
- Immediate/Directed addressing mode 時，如果數值/地址不超過 disp 的表示範圍 (12 bits)，則使用 format 3
![](https://i.imgur.com/GUppSLD.png)
    *disp: displacement* 

### **Format 4** (4 byte)
- 在 Immediate/Directed addressing mode，如果數值/地址無法用 12-bits 表示，則會使用 format 4
![](https://i.imgur.com/qfjydIc.png)
    Bit ***e*** is used to **distinguish between Format 3 and 4**
    - **0: format 3**
    - **1: format 4**

---
### <font>NIXBPE</font>
- **n:** i**N**direct addressing flag
- **i:** **I**mmediate addressing flag
- **x:** inde**X**ed addressing flag
- **b:** **B**ase address relative flag
- **p:** **P**rogram counter relative flag
- **e:** format 4 instruction flag

## Addressing Modes
### BP
#### Format 3
- Two new **relative** addressing modes

    - **Base relative:** <font>b=1, p=0</font>, **TA=(B)+disp**
    *B: Base Register*
        - $0 \le \text{disp} \le 4095$

    - **Program-Counter relative:** <font>b=0, p=1</font>, **TA=(PC)+disp**
    *PC: Program Counter*
        - $-2048 \le \text{disp} \le 2047$ using 2's complement notation
        - **因為程式執行可能會有像迴圈要往回跳的狀況，所以需要負數**

- **Direct addressing**: <font>b=0, p=0</font>, **TA=disp**
> b, p 不可能同時為1

#### Format 4
- **bits b and p are normally set to 0**
    - **Direct addressing: TA=address**

Any of above addressing modes can also be combined with indexed addressing

---
## NI
### Format 3/4 通用

- <font>n=0, i=1</font>, the **target address** itself is used as the **operand value**
    - <font>*Immediate* addressing</font>
    - 算出來的 TA 本身就是資料，而不是地址
    - 常用在**資料是常數**的時候
    - No memory reference is performed

- <font>n=1, i=0</font>, word at the location given by the target address is fetched
    - <font>iNdirect addressing</font>
    - **The value** contained in this word is then taken as **the *address* of the operand value**
    - 類似 C 語言裡面的對指標取值

- <font>n=0, i=0</font> or <font>n=1, i=1</font>, the target address is taken as the location of the operand
    - <font>*Simple* addressing</font>
    - 最一般的使用方法，存放在記憶體 TA 位置的資料即運算元
    - SIC/XE instructions that specify neither *immediate* nor *indirect* addressing are assembled with **bits *n* and *i* both set to 1**. (for upward compatibility)
    - Assemblers for the standard version of SIC will set the bits in both of these positions to 0. This is because **the 8-bit binary codes for all the SIC instructions end in 00.**

**For SIC/XE, if bits n and i are both 0, then bits b, p, and e are considered to be part of the address field of the instruction.**
- This makes Instruction Format 3 identical to the format used on the standard version of SIC, providing the desired compatibility.

:::danger
- 當 **n = 0; i = 0:** 
    - **SIC/XE** 為了向上相容，會改變為 **SIC** 的形式，此時的 ***b, p, e*** 會和 ***disp*** 合併為15個bits
    - 因為會轉變為 **SIC** 的形式，所以 **format 4** 裡 ***n, i*** 不會同時為0
- 當 **n = 1; i = 1:** 
    - 仍會去判斷 ***b, p, e*** 的樣式
:::

:::info
- **Direct/Relative** Addressing Mode 定義如何求出 *TA*
- **Immediate/Indirect/Simple** Addressing Mode 定義求出 *TA* 後，如何得到運算元
- 上面兩種模式是同時存在的，所以一共有 6 種組合，以下舉例三種
    - 一條 format3 指令是 Direct+Simple
        - TA = disp
        - 代表運算元是存在記憶體位置 disp 上的資料
    - 一條 format3 指令是 Direct+Immediate
        - TA = disp
        - 代表運算元是 disp
    - 一條 format3 指令是 Relative+Immediate
        - TA = (B) + disp
        - 代表運算元是 (B) + disp
:::

## Instruction Set

- **Load/Store**: `LDB`, `STB`, ...
- **Floating-Point Number Arithmetic**: `ADDF`, `SUBF`, `MULF`, `DIVF`
- **Register Move**: `RMO`
- **Register-to-Register Arithmetic**: `ADDR`, `SUBR`, `MULR`, `DIVR`
- **Supervisor call instruction**: `SVC`
    - generating an **interrupt** that can be used for **communication with the OS**.

## Input and Output

- There are **I/O channels** that can be used to perform input and output while the CPU is executing other instructions.
- This allow overlap of computing and I/O, resulting in more efficient system operation.
- **Instructions**:
    - **Start I/O:** `SIO`
    - **Test I/O:** `TIO`
    - **Halt I/O:** `HIO`