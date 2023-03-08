---
tags: System Software
---

<style>
    font {
        color: red;
        font-weight: bold;
    }
</style>

# SIC 範例程式分析

- [Appendix A](https://hackmd.io/@coding-guy/B1rBzH30o)
- High Level Language 的部分是 pseudo-code
    - 僅能代表大概相同的邏輯
    - 不保證這段 code 編譯成組合語言會和範例完全一樣

:::info
**判斷 format 的方法**
- **Relative Addressing**: 必為 format 3
- **Immediate Addressing**: 看常數大小
    - 因為 format 3 的 disp 欄位有 12 bits，僅能表示 $-2^{12}$ ~ $2^{12}-1$
- **Memory Access**:  通常是 format 3
- **Operation 前有加號** : 通常是 format 4
:::

:::info
**判斷是否存取記憶體的方法**
- 在語法描述中，`m` 表記憶體位置，`(m)` 是此位置的資料
    - 所以如果描述中有 `(m)` 表示有 memory access
:::

## 1.2 Simple Data Movement

- There are **no memory-to-memory move instructions**
- All data movement must be **done using registers**.
    - 先用 Load 指令把資料從某段記憶體寫入 register
    - 再用 Store 指令把資料從 register 寫入目標記憶體位置
- There are four different ways of defining storage for data items in the SIC assembler language: `WORD`, `RESW`, `BYTE`, `RESB`

### SIC

```=
	LDA 	FIVE
	STA 	ALPHA
	LDCH	CHARZ
	STCH	C1
	 .
	 .
	 .
ALPHA	RESW	1	//宣告變數，長度沒有限制
FIVE	WORD	5	//宣告常數
CHARZ	BYTE	C'Z'	//宣告byte
C1	RESB	1
```

- **Memory usage**: //記憶體使用多少 byte(空間)
    - **Data**: 2 words + 2 bytes = 6 + 2 bytes = 8 bytes
    - **Instruction**: 4 * 3(24 bits) = 12 bytes
    - **Total**: 8 + 12 bytes = 20 bytes
- **\# of memory access**: //記憶體存取幾次(速度)
    - **Data Access**: 4
    - **Instruction Fetch**: 4
    - **Total**: 8

### SIC/XE

```=
	LDA	 #5
	STA	 ALPHA
	LDA	 #90
	STCH	 C1
	 .
	 .
	 .
ALPHA	RESW	1
C1	RESB	1
```

- **Memory usage**:
    - **Data**: 1 word + 1 bytes = 3 + 1 bytes = 4 bytes
    - **Instruction**: 3 + 3 + 3 + 3 = 12 bytes
    - **Total**: 16 bytes
- **\# of memory access**:
    - **Data Access**: 2
    - **Instruction Fetch**: 4
    - **Total**: 6

### High Level Language

```c
int ALPHA;
char C1;

ALPHA = 5;
C1 = 'Z'; 
```

## 1.3 Simple Arithmetic Operation

### SIC

```=
	LDA	ALPHA
	ADD	INCR
	SUB 	ONE
	STA	BETA
	LDA	GAMMA
	ADD 	INCR
	SUB	ONE
	STA	DELTA
	 .
	 .
	 .
ONE	WORD	1
         .
ALPHA	RESW	1
BETA	RESW	1
GAMMA	RESW	1
DELTA	RESW	1
INCR	RESW	1 
```

- **Memory usage**:
    - **Data**: 6 words(3 bytes) = 18 bytes
    - **Instruction**: 8 * 3 = 24 bytes
    - **Total**: 42 bytes
- **\# of memory access**:
    - **Data Access**: 8
    - **Instruction Fetch**: 8
    - **Total**: 16

### SIC/XE

- <font>`ADDR`</font> is used to **avoid having to fetch** `INCR` from memory each time it is used in a calculation, which may make the program more efficient.

```=
	LDS	 INCR
	LDA	 ALPHA
	ADDR	 S,A
	SUB	 #1
	STA 	 BETA
	LDA	 GAMMA
	ADDR	 S,A
	SUB	 #1
	STA	 DELTA
	 .
	 .
	 .
	 .
ALPHA	RESW	1
BETA	RESW	1
GAMMA	RESW	1
DELTA	RESW	1
INCR	RESW	1 
```
:::success
**Q:** `LDS` 和 `LDA` load 兩個變數存進 register 有沒有好處?

**A:** 是有的，因為 `INCR` 在整個程式中使用超過一次，所以先 load 到 register 裡面可以減少 memory access 的次數
:::

- **Memory usage**:
    - **Data**: 5 words = 15 bytes
    - **Instruction**: 2 byte(`ADDR`) * 2 + 3 * 7 = 25 bytes
    - **Total**: 40 bytes
- **\# of memory access**:
    - **Data Access**: 5
    - **Instruction Fetch**: 9
    - **Total**: 14

### High Level Language

```c
int BETA = ALPHA + INCR - 1;
int DELTA = GAMMA + INCR - 1;
```

## 1.4 Simple Looping and Indexing Operations

### SIC

```=
	LDX	ZERO
MOVECH	LDCH	STR1,X
	STCH	STR2,X
	TIX	ELEVEN
	JLT	MOVECH
	 .
	 .
	 .
STR1	BYTE	C'TEST STRING'
STR2 	RESB	11
.
ZERO	WORD	0
ELEVEN	WORD	11
```

- **Memory usage**: 
    - **Data**: 2 words + 11 * 2 bytes = 6 + 22 bytes = 28 bytes
    - **Instruction**: 3 * 5 = 15 bytes
    - **Total**: 43 bytes
- **\# of memory access**:
    - **Data Access**: 1 + 3 * 11 = 34
    - **Instruction Fetch**: 1 + 4 * 11 = 45
    - **Total**: 79

### SIC/XE

- `TIXR` makes the loop **more efficient**
- Because the value does not have to be fetched from memory each time the loop is executed.

```=
	LDT	#11
	LDX	#0
MOVECH	LDCH	STR1,X
	STCH	STR2,X
	TIXR	T
	JLT	MOVECH
	 .
	 .
	 .
STR1	BYTE	C'TEST STRING'
STR2 	RESB	11
```

- **Memory usage**:
    - **Data**: 11 * 2 bytes = 22 bytes
    - **Instruction**: ==2 (`TIXR`)== + 5 * 3 bytes= 17 bytes
    - **Total**: 39 bytes
- **\# of memory access**: 
    - **Data Access**: 2 * 11 = 2 + 22 = 24
    - **Instruction Fetch**: 4 * 11 = 44
    - **Total**: 68

### High Level Language

```c
char STR1 = "TEST STRING";
char STR2[11];
int X = 0;

do{
    STR2[X] = STR1[X]; 
    X++;
}while(X < 11);
```

## 1.5 Sample Indexing and Looping Operations

- The value in the index register must be **incremented by 3** for each iteration of this loop
    - Because each iteration **processes a 3-byte element** of the arrays

### SIC

- The `TIX` instruction always **adds 1 to register X**, so it is <font>not suitable</font> for this program fragment.

- 因為一個 word 占用 3bytes，所以 X 每次要跳三個位置 (X 每次加 3)，才能正確的讀取每一個 word

```=
	LDA	ZERO
	STA	INDEX
ADDLP	LDX	INDEX
	LDA	ALPHA,X
	ADD	BETA,X
	STA	GAMMA,X
	LDA	INDEX
	ADD	THREE
	STA	INDEX
	COMP	K300
	JLT	ADDLP
	 .
	 .
	 .
INDEX	RESW	1
	 .
ALPHA	RESW	100
BETA	RESW	100
GAMMA	RESW	100
	 .
ZERO	WORD	0
K300	WORD	300
THREE	WORD	3
```

:::danger
**Q:** 為什麼程式碼 7 ~ 10 不用 `TIX` ?

**A:** 因為這裡的變數宣告是 word，而不是 byte，要找到下一個資料位置，要加 3 bytes(one word)，而 `TIX` 裡面的內容是，加 1 並比較。
:::

- **Memory usage**:
    - **Data**: 1 + 100*3 + 3 words = 304 words = 912 bytes
    - **Instruction**: 11 * 3 = 33 bytes
    - **Total**: 945 bytes
- **\# of memory access**:
    - **Data Access**: 2 + 8 * 100 = 802
    - **Instruction Fetch**: 2 + 9 * 100 = 902
    - **Total**: 1704

### SIC/XE

```=
	LDS	#3
	LDT	#300
	LDX	#0
ADDLP	LDA	ALPHA,X
	ADD	BETA,X
	STA	GAMMA,X
	ADDR	S,X
	COMPR 	X,T
	JLT	ADDLP
	 .
	 .
	 .
	 .
ALPHA	RESW	100
BETA	RESW	100
GAMMA	RESW	100
```

- **Memory usage**:
    - **Data**: 3 * 100 words = 300 words = 900 bytes
    - **Instruction**: ==2(`ADDR`)== + ==2(`COMPR`)== + 7 * 3 = 25
    - **Total**: 925 bytes
- **\# of memory access**:
    - **Data Access**: 3 * 100 = 300
    - **Instruction Fetch**: 3 + 6 * 100 = 603
    - **Total**: 903

### High Level Language

```c
int ALPHA[100];
int BETA[100];
int GAMMA[100];

for (X = 0; X < 100; X++) {
    GAMMA[X] = ALPHA[X] + BETA[X];
}
```

## 1.6 Simple I/O Operations

### SIC

```=
INLOOP	 TD 	INDEV
	 JEQ	INLOOP
	 RD	INDEV
	 STCH	DATA
	  .
	  .
	  .
OUTLP	 TD	OUTDEV
	 JEQ	OUTLP
	 LDCH	DATA
	 WD	OUTDEV
	  .
	  .
	  .
INDEV	 BYTE	X'F1'	// X 代表使用16進位, 'F1' 是給此裝置的編號
OUTDEV	 BYTE	X'05'
DATA	 RESB	1
```

- **Memory usage**:
    - **Data**: 3 bytes
    - **Instruction**: (4 + 4) * 3 = 24 bytes
    - **Total**: 27 bytes

<br>

> 國展表示這邊 access 不用算 //TD RD WD
- **\# of memory access**:
    - **Data Access**: 2
    - **Instruction Fetch**: 2n + 2m + 4 (n, m: 測試 device 的次數)
    - **Total**: 2n + 2m + 6

### High Level Language

```cpp
Device INDEV = Device("F1");
Device OUTDEV = Device("05");

char DATA;

// 不能使用就繼續測試
while (! INDEV.isAvailable()) {
}
DATA = INDEV.read();

    .
    .
    .

while (! OUTDEV.isAvailable()) {
}
OUTDEV.write(DATA);
```

## 1.7 Sample Subroutine Call and Record Input Operations

### SIC

```=
	JSUB	READ
	 .
	 .
	 .
READ	LDX	ZERO
RLOOP	TD	INDEV
	JEQ	RLOOP
	RD	INDEV
	STCH	RECORD,X
	TIX	K100
	JLT	RLOOP
	RSUB
	 .
	 .	
	 .
INDEV	BYTE	X'F1'
RECORD	RESB	100
	 .	
ZERO	WORD	0
K100	WORD	100
```

- **Memory usage**:
    - **Data**: (1 + 100) bytes + 2 words = 107 bytes
    - **Instruction**: 9 * 3 = 27 bytes
    - **Total**: 134 bytes

> 國展表示這邊 access 不用算 //TD RD WD
- **\# of memory access**:
    - **Data Access**: 1 + 2 * 100 = 201
    - **Instruction Fetch**: 1 + 1 + 2n + 5 * 100 + 1 (n: 測試 device 的次數)
    - **Total**: 2n + 704

### SIC/XE

```=
	JSUB	READ
	 .
	 .
	 .
READ	LDX	#0
	LDT	#100
RLOOP	TD	 INDEV
	JEQ	 RLOOP
	RD	 INDEV
	STCH	 RECORD,X
	TIXR	 T
	JLT	 RLOOP
	RSUB
	 .
	 .
	 .
INDEV	BYTE	X'F1'
RECORD	RESB	100
```

- **Memory usage**:
    - **Data**: (1 + 100) bytes = 101 bytes
    - **Instruction**: ==2(`TIXR`)== + 3 * 9 = 29 bytes
    - **Total**: 130 bytes

> 國展表示這邊 access 不用算 //TD RD WD
- **Memory Access**

### High Level Language

```cpp
Device INDEV = Device("F1");
char RECORD[100];

READ();

void READ() {
    for(X = 0; X < 100; X++) {
        while (! INDEV.isAvailable()) {
        }
        RECORD[X] = INDEV.read();
    }
}

```
