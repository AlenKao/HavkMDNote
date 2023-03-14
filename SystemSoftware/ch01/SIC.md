###### tags: `System Software` 
<style>
    font {
        color: red;
        font-weight: bold;
    }
</style>

# SIC Machine Architecture

## [Memory](https://www.youtube.com/watch?v=2lSyHZLzNYA)

- **Byte**: **8-bit**
- **Word**: consecutive **3 bytes** (24 bits).
- All addresses are **byte addresses**
- **Words** are **addressed** by the location of their **lowest numbered byte**.
- There are a total of **32768 ($2^{15}$) bytes** in the computer memory.
    - Address 的範圍: $0$ ~ $2^{15} - 1$

## <font>Registers</font>
:::danger
Five registers, all of which have special uses. Each register is **24 bits** in length.
- **A:** **Accumulator** 累加器
- **X:** **Index register**
- **L:** **Linkage register**; storing the return address after a funciton call. 
- **PC:** **Program Counter** 儲存下一條要執行的指令(在機器語言中)的記憶體位址
- **SW:** **Status Word**; including a **Condition Code (CC)**
:::

## Data formats

- **Integers** are stored as **24-bit** binary **2's complement** representation is used for negative values.
    - 二補數相較於一補數沒有 +0 與 -0 的問題，可以多表達一個數。

- **Characters** are stored using their **8-bit ASCII codes**.

- There is **no floating-point** hardware on the standard version of SIC.

## Instruction formats
![](https://i.imgur.com/YzynDzE.png)

- ==**24-bit** format==
    - ***opcode (指令碼):*** **8bits**, 代表要執行的是哪種指令
    
    - ***x:*** **1bit**, 用來表示是否為索引定址模式 (**indicated indexed-addressing mode**)
    
    - ***address:*** **15bits**, 因為記憶體大小為 $2^{15}$bytes, 因此15個bits就能表示所有記憶體位置, 存在此位置的資料就是此指令要使用的資料

## Addressing modes

- There are two addressing mode avilable.
- **Direct: x = 0; TA (target address) = address**
- **Indexed: x = 1; TA = address + (x)**
    - **x** is the **index register** (*x* register)
- Parentheses (小括號) are used to indicate the contents of a register or a memory location.

## Instruction set

- **Load and store registers**
    - `LDA`, `LDX`, ... : Load *A*, Load *X*, etc.
    - `STA`, `STX`, ... : Store *A*, Store *X*, etc.

- **Integer arithmetic** operations
    - `ADD`, `SUB`, `MUL`, `DIV`

- `COMP`: **Compares** the **value in register *A*** with a **word in memory**
    - <font>Setting</font> a **condition code (CC)** to indicate the result (<, =, or >)
    - **CC is stored in register *SW***

- **Conditional jump instructions**
    - `JLT`: Jump if CC is set to < (**L**ess **T**han)
    - `JEQ`: Jump if CC is set to = (**EQ**uals to)
    - `JGT`: Jump if CC is set to > (**G**reater **T**han)
    :::info
    在高階語言中，下列的敘述是: *如果 a > b，則向下執行 X 區段，**否**則**跳**到Y區段*
    ```cpp
    if (a > b) {
        /* ===== X 區段 ===== */
            ...
        /* ===== X 區段 ===== */
    } else {
        /* ===== Y 區段 ===== */
            ...
        /* ===== Y 區段 ===== */
    }
    ```
   
    但在低階語言中，JUMP 的指令(`JLE`, `JGT`, `JLT`)是**滿足**某些 condition 才會跳到指定區段，和高階語言的邏輯相反
    
    所以編譯器把高階語言轉換成低階語言時，會把上面那段程式碼中，X 和 Y 區段對調，以便用低階語言實作相同的邏輯
    
    即，*如果 a > b，則跳到 X 區段，否則繼續下向執行 Y 區段*
    
    ```
    COMP a, b
    JGT X
    /* ===== Y 區段 ===== */
        ...
    /* ===== Y 區段 ===== */
    
    /* ===== X 區段 ===== */
        ...
    /* ===== X 區段 ===== */
    ```
    :::
- ==`JSUB`==: **jumping to a subroutine (function call)**

- ==`RSUB`==: **returning from a subroutine to the address contained in register *L***

## Input & Output

- 在標準版本的 SIC 中，進行 I/O 時會**從 regiter *A*** 的**最右的 8 個 bits** 開始(rightmost 8 bits)，**一次搬動一個 byte**
- Each device is assigned a unique **8-bit code**
    - 10 進位表示: `0` ~ `255`
    - 16 進位表示: `00` ~ `FF`
- There are three I/O instructions:
    - **Test Device**: `TD`
    - **Read Data**: `RD`
    - **Write Data**: `WD`
- On executing `TD`, the **condition code** is set to indicate the **result of test**
    - **<** means device is **ready** to send or receive
    - **=** means device is **not ready**