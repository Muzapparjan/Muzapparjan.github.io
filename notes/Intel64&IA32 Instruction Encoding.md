# Intel64 and IA32 Instruction Encoding

## Preface

* Date: 07/11/2023
* Reference: Intel® 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes - Volume 2 - Chapter 2 Instruction Format
* Link: [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)

## Overview

The Intel 64 and IA-32 architectures instruction encodings are subsets of the format shown in below.

|Prefixes|Opcode|ModR/M|SIB|Displacement|Immediate|
|-|-|-|-|-|-|
|Prefixes of 1 byte each(optional)|Opcode of 1/2/3 bytes|1 byte(if required)|1 byte(if required)|Address displacement of 1/2/4 bytes or none|Immediate data of 1/2/4 bytes or none|

|[7-6]|[5-3]|[2-0]|
|-|-|-|
|Mod|Reg/Opcode|R/M|

|[7-6]|[5-3]|[2-0]|
|-|-|-|
|Scale|Index|Base|

Instructions consist of optional instruction prefixes (in any order), primary opcode bytes (up to three bytes), an addressing-form specifier (if required)consisting of the ModR/M byte and sometimes the SIB (Scale-Index-Base) byte, a displacement (if required), and an immediate data field (if required).

## Prefixes

Instruction prefixes are divided into four groups, each with a set of allowable prefix codes. For each instruction, it is only useful to include up to one prefix code from each of the four groups (Groups 1, 2, 3, 4). Groups 1 through 4 may be placed in any order relative to each other.

* Group 1
  * Lock and repeat prefixes:
    * **LOCK** prefix is encoded using **F0H**.
    * **REPNE/REPNZ** prefix is encoded using **F2H**. Repeat-Not-Zero prefix applies only to string and input/output instructions. (F2H is also used as a mandatory prefix for some instructions.)
    * **REP** or **REPE/REPZ** is encoded using **F3H**. The repeat prefix applies only to string and input/output instructions. (F3H is also used as a mandatory prefix for some instructions.)
  * **BND** prefix is encoded using **F2H** if the following conditions are true:
    * CPUID.(EAX=07H, ECX=0):EBX.MPX[bit 14] is set.
    * BNDCFGU.EN and/or IA32_BNDCFGS.EN is set.
    * When the F2 prefix precedes a near CALL, a near RET, a near JMP, a short Jcc, or a near Jcc instruction (see Appendix E, “Intel® Memory Protection Extensions,” of the Intel® 64 and IA-32 Architectures Software Developer’s Manual, Volume 1).
* Group 2
  *  Segment override prefixes:
     *  **2EH—CS** segment override (use with any branch instruction is reserved).
     *  **36H—SS** segment override prefix (use with any branch instruction is reserved).
     *  **3EH—DS** segment override prefix (use with any branch instruction is reserved).
     *  **26H—ES** segment override prefix (use with any branch instruction is reserved).
     *  **64H—FS** segment override prefix (use with any branch instruction is reserved).
     *  **65H—GS** segment override prefix (use with any branch instruction is reserved).
  * Branch hints:
    * **2EH**—Branch not taken (used only with Jcc instructions).
    * **3EH**—Branch taken (used only with Jcc instructions).
* Group 3
  * **Operand-size override** prefix is encoded using **66H** (66H is also used as a mandatory prefix for some instructions).
* Group 4
  * **67H**—**Address-size override** prefix.

## Opcodes

A primary opcode can be **1, 2, or 3** bytes in length. An **additional 3-bit** opcode field is sometimes encoded in the **ModR/M** byte. Smaller fields can be defined within the primary opcode. Such fields define the direction of operation, size of displacements, register encoding, condition codes, or sign extension. Encoding fields used by an opcode vary depending on the class of operation.

Two-byte opcode formats for general-purpose and SIMD instructions consist of one of the following:

* An escape opcode byte **0FH** as the primary opcode and a second opcode byte.
* A mandatory prefix (**66H, F2H, or F3H**), an escape opcode byte, and a second opcode byte (same as previous bullet).

## ModR/M and SIB Bytes

Many instructions that refer to an operand in memory have an addressing-form specifier byte (called the ModR/M byte) following the primary opcode. The ModR/M byte contains three fields of information:

* The mod field combines with the r/m field to form **32** possible values: **eight registers and 24 addressing modes**.
* The reg/opcode field specifies either a register number or three more bits of opcode information. The purpose of the reg/opcode field is specified in the primary opcode.

Certain encodings of the ModR/M byte require a second addressing byte (the SIB byte). The base-plus-index and scale-plus-index forms of 32-bit addressing require the SIB byte. The SIB byte includes the following fields:

* The scale field specifies the scale factor.
* The index field specifies the register number of the index register.
* The base field specifies the register number of the base register.

## Displacement and Immediate Bytes

Some addressing forms include a displacement immediately following the ModR/M byte (or the SIB byte if one is present). If a displacement is required, it can be 1, 2, or 4 bytes.

If an instruction specifies an immediate operand, the operand always follows any displacement bytes. An immediate operand can be 1, 2 or 4 bytes.

## A Cat Passed by Here

```
    /\_____/\
   /  o   o  \
  ( ==  ^  == )
   )         (
  (           )
 ( (  )   (  ) )
(__(__)___(__)__)
```

## OK LET'S GO ONE STEP DEEPER

## Yet Another Preface

* Date: 08/11/2023
* Reference: Intel® 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes - Volume 2 - Appendix B Instruction Formats and Encodings
* Link: [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)

## Overview

All Intel Architecture instructions are encoded using subsets of the general machine instruction format shown below. Each instruction consists of:

* an opcode
* a register and/or address mode specifier consisting of the ModR/M byte and sometimes the scale-index-base (SIB) byte (if required)
* a displacement and an immediate data field (if required)

|Legacy Prefixes|REX Prefixes|Opcodes|ModR/M|SIB|Displacement|Immediate|
|-|-|-|-|-|-|-|
|Grp1/2/3/4|optional|1-,2-, or 3- bytes|Mod-Reg-R/M|Scale-Index-Base|Address Displacement(1,2,4 bytes or none)|Immediate Data(1,2,4 bytes or none)

Note: 

* The Reg Field may be used as an opcode extension field (TTT) and as a way to encode diagnostic registers (eee).

## Legacy Prefixes

The legacy prefixes include **66H, 67H, F2H, and F3H**. They are optional, except when **F2H, F3H, and 66H** are used in instruction extensions. Legacy prefixes must be placed before REX prefixes.

## REX Prefixes

REX prefixes are a set of 16 opcodes that span one row of the opcode map and occupy **entries 40H to 4FH**. These opcodes represent valid instructions (**INC or DEC**) in IA-32 operating modes and in compatibility mode. In 64-bit mode, the same opcodes represent the instruction prefix REX and are **not treated as individual instructions**.

## Opcode Fields

The primary opcode for an instruction is encoded in one to three bytes of the instruction. Within the primary opcode, smaller encoding fields may be defined. These fields vary according to the class of operation being performed.

Almost all instructions that refer to a register and/or memory operand have a register and/or address mode byte following the opcode. This byte, the ModR/M byte, consists of the mod field (2 bits), the reg field (3 bits; this field is sometimes an opcode extension), and the R/M field (3 bits). Certain encodings of the ModR/M byte indicate that a second address mode byte, the SIB byte, must be used.

If the addressing mode specifies a displacement, the displacement value is placed immediately following the ModR/M byte or SIB byte. Possible sizes are 8, 16, or 32 bits. If the instruction specifies an immediate value, the immediate value follows any displacement bytes. The immediate, if specified, is always the last field of the instruction.

## Special Fields

Table below lists bit fields that appear in certain instructions, sometimes within the opcode bytes.

|Field Name|Description|Number of Bits|
|:-:|:-|:-:|
|reg|General Register Specifier|3|
|w|Specifies if data is byte or full-sized, where full-sized is 16 or 32 bits|1|
|s|Specifies sign extension of an immediate field|1|
|sreg2|Segment register specifier for CS, SS, DS, ES|2|
|sreg3|Segment register specifier for CS, SS, DS, ES, FS, GS|3|
|eee|Specifies a special-purpose (control or debug) register|3|
|tttn|For conditional instructions, specifies a condition asserted or negated|4|
|d|Specifies direction of data operation|1|

## Reg Field (reg) for Non-64-Bit Modes

The reg field in the ModR/M byte specifies a general-purpose register operand. The group of registers specified is modified by the presence and state of the w bit in an encoding (refer to Section B.1.4.3). Tables below show the encoding of the reg field when the w bit is not present in an encoding, and the encoding of the reg field when the w bit is present.

|reg Field|Register Selected during 16-Bit Data Operations|Register Selected during 32-Bit Data Operations|
|:-:|:-:|:-:|
|000|AX|EAX|
|001|CX|ECX|
|010|DX|EDX|
|011|BX|EBX|
|100|SP|ESP|
|101|BP|EBP|
|110|SI|ESI|
|111|DI|EDI|

<table style="text-align:center">
    <tr>
        <td colspan="3">Register Specified by reg Field During 16-Bit Data Operations</td>
    </tr>
    <tr>
        <td rowspan="2">reg</td>
        <td colspan="2">Function of w Field</td>
    </tr>
    <tr>
        <td>When w = 0</td>
        <td>When w = 1</td>
    </tr>
    <tr>
        <td>000</td>
        <td>AL</td>
        <td>AX</td>
    </tr>
    <tr>
        <td>001</td>
        <td>CL</td>
        <td>CX</td>
    </tr>
    <tr>
        <td>010</td>
        <td>DL</td>
        <td>DX</td>
    </tr>
    <tr>
        <td>011</td>
        <td>BL</td>
        <td>BX</td>
    </tr>
    <tr>
        <td>100</td>
        <td>AH</td>
        <td>SP</td>
    </tr>
    <tr>
        <td>101</td>
        <td>CH</td>
        <td>BP</td>
    </tr>
    <tr>
        <td>110</td>
        <td>DH</td>
        <td>SI</td>
    </tr>
    <tr>
        <td>111</td>
        <td>BH</td>
        <td>DI</td>
    </tr>
</table>

<table style="text-align:center">
    <tr>
        <td colspan="3">Register Specified by reg Field During 32-Bit Data Operations</td>
    </tr>
    <tr>
        <td rowspan="2">reg</td>
        <td colspan="2">Function of w Field</td>
    </tr>
    <tr>
        <td>When w = 0</td>
        <td>When w = 1</td>
    </tr>
    <tr>
        <td>000</td>
        <td>AL</td>
        <td>EAX</td>
    </tr>
    <tr>
        <td>001</td>
        <td>CL</td>
        <td>ECX</td>
    </tr>
    <tr>
        <td>010</td>
        <td>DL</td>
        <td>EDX</td>
    </tr>
    <tr>
        <td>011</td>
        <td>BL</td>
        <td>EBX</td>
    </tr>
    <tr>
        <td>100</td>
        <td>AH</td>
        <td>ESP</td>
    </tr>
    <tr>
        <td>101</td>
        <td>CH</td>
        <td>EBP</td>
    </tr>
    <tr>
        <td>110</td>
        <td>DH</td>
        <td>ESI</td>
    </tr>
    <tr>
        <td>111</td>
        <td>BH</td>
        <td>EDI</td>
    </tr>
</table>

## Reg Field (reg) for 64-Bit Mode

Just like in non-64-bit modes, the reg field in the ModR/M byte specifies a general-purpose register operand. The group of registers specified is modified by the presence of and state of the w bit in an encoding (refer to Section 
B.1.4.3). Tables below show the encoding of the reg field when the w bit is not present in an encoding, and the encoding of the reg field when the w bit is present.

|reg Field|Register Selected during 16-Bit Data Operations|Register Selected during 32-Bit Data Operations|Register Selected during 64-Bit Data Operations|
|:-:|:-:|:-:|:-:|
|000|AX|EAX|RAX|
|001|CX|ECX|RCX|
|010|DX|EDX|RDX|
|011|BX|EBX|RBX|
|100|SP|ESP|RSP|
|101|BP|EBP|RBP|
|110|SI|ESI|RSI|
|111|DI|EDI|RDI|

<table style="text-align:center">
    <tr>
        <td colspan="3">Register Specified by reg Field During 16-Bit Data Operations</td>
    </tr>
    <tr>
        <td rowspan="2">reg</td>
        <td colspan="2">Function of w Field</td>
    </tr>
    <tr>
        <td>When w = 0</td>
        <td>When w = 1</td>
    </tr>
    <tr>
        <td>000</td>
        <td>AL</td>
        <td>AX</td>
    </tr>
    <tr>
        <td>001</td>
        <td>CL</td>
        <td>CX</td>
    </tr>
    <tr>
        <td>010</td>
        <td>DL</td>
        <td>DX</td>
    </tr>
    <tr>
        <td>011</td>
        <td>BL</td>
        <td>BX</td>
    </tr>
    <tr>
        <td>100</td>
        <td>AH</td>
        <td>SP</td>
    </tr>
    <tr>
        <td>101</td>
        <td>CH</td>
        <td>BP</td>
    </tr>
    <tr>
        <td>110</td>
        <td>DH</td>
        <td>SI</td>
    </tr>
    <tr>
        <td>111</td>
        <td>BH</td>
        <td>DI</td>
    </tr>
</table>

<table style="text-align:center">
    <tr>
        <td colspan="3">Register Specified by reg Field During 32-Bit Data Operations</td>
    </tr>
    <tr>
        <td rowspan="2">reg</td>
        <td colspan="2">Function of w Field</td>
    </tr>
    <tr>
        <td>When w = 0</td>
        <td>When w = 1</td>
    </tr>
    <tr>
        <td>000</td>
        <td>AL</td>
        <td>EAX</td>
    </tr>
    <tr>
        <td>001</td>
        <td>CL</td>
        <td>ECX</td>
    </tr>
    <tr>
        <td>010</td>
        <td>DL</td>
        <td>EDX</td>
    </tr>
    <tr>
        <td>011</td>
        <td>BL</td>
        <td>EBX</td>
    </tr>
    <tr>
        <td>100</td>
        <td>AH</td>
        <td>ESP</td>
    </tr>
    <tr>
        <td>101</td>
        <td>CH</td>
        <td>EBP</td>
    </tr>
    <tr>
        <td>110</td>
        <td>DH</td>
        <td>ESI</td>
    </tr>
    <tr>
        <td>111</td>
        <td>BH</td>
        <td>EDI</td>
    </tr>
</table>

Note: 
  * AH, CH, DH, BH **can not be encoded when REX prefix is used**. Such an expression defaults to the low byte.

## Encoding of Operand Size (w) Bit

The current operand-size attribute determines whether the processor is performing 16-bit, 32-bit or 64-bit operations. Within the constraints of the current operand-size attribute, the operand-size bit (w) can be used to indicate operations on 8-bit operands or the full operand size specified with the operand-size attribute. Table below shows the encoding of the w bit depending on the current operand-size attribute.

|w Bit|Operand Size When Operand-Size Attribute is 16-Bits|Operand Size When Operand-Size Attribute is 32-Bits|
|:-:|:-:|:-:|
|0|8 Bits|8 Bits|
|1|16 Bits|32 Bits|

## Sign-Extend (s) Bit

The sign-extend (s) bit occurs in instructions with immediate data fields that are being extended from 8 bits to 16 or 32 bits.

|s|Effect on 8-Bit Immediate Data|Effect ib 16- or 32-Bit Immediate Data|
|:-:|:-:|:-:|
|0|None|None|
|1|Sign-extend to fill 16-bit or 32-bit destination|None|

## Segment Register (sreg) Field

When an instruction operates on a segment register, the reg field in the ModR/M byte is called the sreg field and is used to specify the segment register. Table below shows the encoding of the sreg field. This field is sometimes a 2-bit field (sreg2) and other times a 3-bit field (sreg3).

|2-Bit sreg2 Field| Segment Register Selected|
|:-:|:-:|
|00|ES|
|01|CS|
|10|SS|
|11|DS|

|3-Bit sreg3 Field| Segment Register Selected|
|:-:|:-:|
|000|ES|
|001|CS|
|010|SS|
|011|DS|
|100|FS|
|101|GS|
|110|Reserved|
|111|Reserved|

Note:
  * Do not use reserved encodings.

## Special-Purpose Register (eee) Field

When control or debug registers are referenced in an instruction they are encoded in the eee field, located in bits 5 though 3 of the ModR/M byte (an alternate encoding of the sreg field).

|eee|Control Register|Debug Register|
|:-:|:-:|:-:|
|000|CR0|DR0|
|001|Reserved|DR1|
|010|CR2|DR2|
|011|CR3|DR3|
|100|CR4|Reserved|
|101|Reserved|Reserved|
|110|Reserved|DR6|
|111|Reserved|DR7|

## Condition Test (tttn) Field

For conditional instructions (such as conditional jumps and set on condition), the condition test field (tttn) is encoded for the condition being tested. The ttt part of the field gives the condition to test and the n part indicates whether to use the condition (n = 0) or its negation (n = 1).

* For 1-byte primary opcodes, the tttn field is located in bits 3, 2, 1, and 0 of the opcode byte.
* For 2-byte primary opcodes, the tttn field is located in bits 3, 2, 1, and 0 of the second opcode byte.

|tttn|Mnemonic|Condition|
|:-:|:-|:-|
|0000|O|Overflow|
|0001|NO|No overflow|
|0010|B, NAE|Below, Not above or equal|
|0011|NB, AE|Not below, Above or equal|
|0100|E, Z|Equal, Zero|
|0101|NE, NZ|Not equal, Not zero|
|0110|BE, NA|Below or equal, Not above|
|0111|NBE, A|Not below or equal, Above|
|1000|S|Sign|
|1001|NS|Not sign|
|1010|P, PE|Parity, Parity even|
|1011|NP, PO|Not parity, Parity odd|
|1100|L, NGE|Less than, Not greater than or equal to|
|1101|NL, GE|Not less than, Greater than or equal to|
|1110|LE, NG|Less than or equal to, Not greater than|
|1111|NLE, G|Not less than or equal to, greater than|

## Direction (d) Bit

In many two-operand instructions, a direction bit (d) indicates which operand is considered the source and which is the destination.

* When used for integer instructions, the d bit is located at bit 1 of a 1-byte primary opcode.
* When used for floating-point instructions (in Table B-16), the d bit is shown as bit 2 of the first byte of the primary opcode.

|d|Source|Destination|
|:-:|:-|:-|
|0|reg Field|ModR/M or SIB Field|
|1|ModR/M or SIB Field|reg Field|

## Other Notes

Table below contains notes on particular encodings.

|Symbol|Note|
|:-:|:-|
|A|A value of 11B in bits 7 and 6 of the ModR/M byte is reserved.|
|B|A value of 01B (or 10B) in bits 7 and 6 of the ModR/M byte is reserved.|

## A Cat Passed by Here AGAIN

```
    /\_____/\
   /  o   o  \
  ( ==  ^  == )
   )         (
  (           )
 ( (  )   (  ) )
(__(__)___(__)__)
```

## OK LET'S GO ONE STEP DEEEEEEEEEPER

## The Last Preface

* Date: 09/11/2023
* Reference: Intel® 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes - Volume 2 - Appendix B Instruction Formats and Encodings
* Link: [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)

## GENERAL-PURPOSE INSTRUCTION FORMATS AND ENCODINGS FOR NON-64-BIT MODES

Table below shows machine instruction formats and encodings for general purpose instructions in non-64-bit modes.

|Instruction and Format|Encoding|
|:-|:-|
|**AAA – ASCII Adjust after Addition**|0011 0111|
|**AAD – ASCII Adjust AX before Division**|1101 0101 : 0000 1010|
|**AAM – ASCII Adjust AX after Multiply**|1101 0100 : 0000 1010|
|**AAS – ASCII Adjust AL after Subtraction**|0011 1111|
|**ADC – ADD with Carry**|--------------------------------------------------|
|  register1 to register2|0001 000w : 11 reg1 reg2|
|  register2 to register1|0001 001w : 11 reg1 reg2|
|  memory to register|0001 001w : mod reg r/m|
|  register to memory|0001 000w : mod reg r/m|
|  immediate to register|1000 00sw : 11 010 reg : immediate data|
|  immediate to AL, AX, or EAX|0001 010w : immediate data|
|  immediate to memory|1000 00sw : mod 010 r/m : immediate data|
|**ADD – Add**|--------------------------------------------------|
|  register1 to register2|0000 000w : 11 reg1 reg2|
|  register2 to register1|0000 001w : 11 reg1 reg2|
|  memory to register|0000 001w : mod reg r/m|
|  register to memory|0000 000w : mod reg r/m|
|  immediate to register|1000 00sw : 11 000 reg : immediate data|
|  immediate to AL, AX, or EAX|0000 010w : immediate data|
|  immediate to memory|1000 00sw : mod 000 r/m : immediate data|
|**AND – Logical AND**|--------------------------------------------------|
|  register1 to register2|0010 000w : 11 reg1 reg2|
|  register2 to register1|0010 001w : 11 reg1 reg2|
|  memory to register|0010 001w : mod reg r/m|
|  register to memory|0010 000w : mod reg r/m|
|  immediate to register|1000 00sw : 11 100 reg : immediate data|
|  immediate to AL, AX, or EAX|0010 010w : immediate data|
|  immediate to memory|1000 00sw : mod 100 r/m : immediate data|
|**ARPL – Adjust RPL Field of Selector**|--------------------------------------------------|
|  from register|0110 0011 : 11 reg1 reg2|
|  from memory|0110 0011 : mod reg r/m|
|**BOUND – Check Array Against Bounds**|0110 0010 : modA reg r/m|
|**BSF – Bit Scan Forward**|--------------------------------------------------|
|  register1, register2|0000 1111 : 1011 1100 : 11 reg1 reg2|
|  memory, register|0000 1111 : 1011 1100 : mod reg r/m|
|**BSR – Bit Scan Reverse**|--------------------------------------------------|
|  register1, register2|0000 1111 : 1011 1101 : 11 reg1 reg2|
|  memory, register|0000 1111 : 1011 1101 : mod reg r/m|
|**BSWAP – Byte Swap**|0000 1111 : 1100 1 reg|
|**BT – Bit Test**|--------------------------------------------------|
|  register, immediate|0000 1111 : 1011 1010 : 11 100 reg: imm8 data|
|  memory, immediate|0000 1111 : 1011 1010 : mod 100 r/m : imm8 data|
|  register1, register2|0000 1111 : 1010 0011 : 11 reg2 reg1|
|  memory, reg|0000 1111 : 1010 0011 : mod reg r/m|
|**BTC – Bit Test and Complement**|--------------------------------------------------|
|  register, immediate|0000 1111 : 1011 1010 : 11 111 reg: imm8 data|
|  memory, immediate|0000 1111 : 1011 1010 : mod 111 r/m : imm8 data|
|  register1, register2|0000 1111 : 1011 1011 : 11 reg2 reg1|
|  memory, reg|0000 1111 : 1011 1011 : mod reg r/m|
|**BTR – Bit Test and Reset**|--------------------------------------------------|
|  register, immediate|0000 1111 : 1011 1010 : 11 110 reg: imm8 data|
|  memory, immediate|0000 1111 : 1011 1010 : mod 110 r/m : imm8 data|
|  register1, register2|0000 1111 : 1011 0011 : 11 reg2 reg1|
|  memory, reg|0000 1111 : 1011 0011 : mod reg r/m|
|**BTS – Bit Test and Set**|--------------------------------------------------|
|  register, immediate|0000 1111 : 1011 1010 : 11 101 reg: imm8 data|
|  memory, immediate|0000 1111 : 1011 1010 : mod 101 r/m : imm8 data|
|  register1, register2|0000 1111 : 1010 1011 : 11 reg2 reg1|
|  memory, reg|0000 1111 : 1010 1011 : mod reg r/m|
|**CALL – Call Procedure (in same segment)**|--------------------------------------------------|
|  direct|1110 1000 : full displacement|
|  register indirect|1111 1111 : 11 010 reg|
|  memory indirect|1111 1111 : mod 010 r/m|
|**CALL – Call Procedure (in other segment)**|--------------------------------------------------|
|  direct|1001 1010 : unsigned full offset, selector|
|  indirect|1111 1111 : mod 011 r/m|
|**CBW – Convert Byte to Word**|1001 1000|
|**CDQ – Convert Doubleword to Qword**|1001 1001|
|**CLC – Clear Carry Flag**|1111 1000|
|**CLD – Clear Direction Flag**|1111 1100|
|**CLI – Clear Interrupt Flag**|1111 1010|
|**CLTS – Clear Task-Switched Flag in CR0**|0000 1111 : 0000 0110|
|**CMC – Complement Carry Flag**|1111 0101|
|**CMP – Compare Two Operands**|--------------------------------------------------|
|  register1 with register2|0011 100w : 11 reg1 reg2|
|  register2 with register1|0011 101w : 11 reg1 reg2|
|  memory with register|0011 100w : mod reg r/m|
|  register with memory|0011 101w : mod reg r/m|
|  immediate with register|1000 00sw : 11 111 reg : immediate data|
|  immediate with AL, AX, or EAX|0011 110w : immediate data|
|  immediate with memory|1000 00sw : mod 111 r/m : immediate data|
|**CMPS/CMPSB/CMPSW/CMPSD – Compare String Operands**|1010 011w|
|**CMPXCHG – Compare and Exchange**|--------------------------------------------------|
|  register1, register2|0000 1111 : 1011 000w : 11 reg2 reg1|
|  memory, register|0000 1111 : 1011 000w : mod reg r/m|
|**CPUID – CPU Identification**|0000 1111 : 1010 0010|
|**CWD – Convert Word to Doubleword**|1001 1001|
|**CWDE – Convert Word to Doubleword**|1001 1000|
|**DAA – Decimal Adjust AL after Addition**|0010 0111|
|**DAS – Decimal Adjust AL after Subtraction**|0010 1111|
|**DEC – Decrement by 1**|--------------------------------------------------|
|  register|1111 111w : 11 001 reg|
|  register (alternate encoding)|0100 1 reg|
|  memory|1111 111w : mod 001 r/m|
|**DIV – Unsigned Divide**|--------------------------------------------------|
|  AL, AX, or EAX by register|1111 011w : 11 110 reg|
|  AL, AX, or EAX by memory|1111 011w : mod 110 r/m|
|  HLT – Halt|1111 0100|
|**IDIV – Signed Divide**|--------------------------------------------------|
|  AL, AX, or EAX by register|1111 011w : 11 111 reg|
|  AL, AX, or EAX by memory|1111 011w : mod 111 r/m|
|**IMUL – Signed Multiply**|--------------------------------------------------|
|  AL, AX, or EAX with register|1111 011w : 11 101 reg|
|  AL, AX, or EAX with memory|1111 011w : mod 101 reg|
|  register1 with register2|0000 1111 : 1010 1111 : 11 : reg1 reg2|
|  register with memory|0000 1111 : 1010 1111 : mod reg r/m|
|  register1 with immediate to register2|0110 10s1 : 11 reg1 reg2 : immediate data|
|  memory with immediate to register|0110 10s1 : mod reg r/m : immediate data|
|**IN – Input From Port**|--------------------------------------------------|
|  fixed port|1110 010w : port number|
|  variable port|1110 110w|
|**INC – Increment by 1**|--------------------------------------------------|
|  reg|1111 111w : 11 000 reg|
|  reg (alternate encoding)|0100 0 reg|
|  memory|1111 111w : mod 000 r/m|
|**INS – Input from DX Port**|0110 110w|
|**INT n – Interrupt Type n**|1100 1101 : type|
|**INT – Single-Step Interrupt 3**|1100 1100|
|**INTO – Interrupt 4 on Overflow**|1100 1110|
|**INVD – Invalidate Cache**|0000 1111 : 0000 1000|
|**INVLPG – Invalidate TLB Entry**|0000 1111 : 0000 0001 : mod 111 r/m|
|**INVPCID – Invalidate Process-Context Identifier**|0110 0110:0000 1111:0011 1000:1000 0010: mod reg r/m|
|**IRET/IRETD – Interrupt Return**|1100 1111|
|**Jcc – Jump if Condition is Met**|--------------------------------------------------|
|  8-bit displacement|0111 tttn : 8-bit displacement|
|  full displacement|0000 1111 : 1000 tttn : full displacement|
|**JCXZ/JECXZ – Jump on CX/ECX Zero**|--------------------------------------------------|
|  Address-size prefix differentiates JCXZ and JECXZ| 1110 0011 : 8-bit displacement|
|**JMP – Unconditional Jump (to same segment)**|--------------------------------------------------|
|  short|1110 1011 : 8-bit displacement|
|  direct|1110 1001 : full displacement|
|  register indirect|1111 1111 : 11 100 reg|
|  memory indirect|1111 1111 : mod 100 r/m|
|**JMP – Unconditional Jump (to other segment)**|--------------------------------------------------|
|  direct intersegment|1110 1010 : unsigned full offset, selector|
|  indirect intersegment|1111 1111 : mod 101 r/m|
|**LAHF – Load Flags into AHRegister**|1001 1111|
|**LAR – Load Access Rights Byte**|--------------------------------------------------|
|  from register|0000 1111 : 0000 0010 : 11 reg1 reg2|
|  from memory|0000 1111 : 0000 0010 : mod reg r/m|
|**LDS – Load Pointer to DS**|1100 0101 : modA,B reg r/m|
|**LEA – Load Effective Address**|1000 1101 : modA reg r/m|
|**LEAVE – High Level Procedure Exit**|1100 1001|
|**LES – Load Pointer to ES**|1100 0100 : modA,B reg r/m|
|**LFS – Load Pointer to FS**|0000 1111 : 1011 0100 : modA reg r/m|
|**LGDT – Load Global Descriptor Table Register**|0000 1111 : 0000 0001 : modA 010 r/m|
|**LGS – Load Pointer to GS**|0000 1111 : 1011 0101 : modA reg r/m|
|**LIDT – Load Interrupt Descriptor Table Register**|0000 1111 : 0000 0001 : modA 011 r/m|
|**LLDT – Load Local Descriptor Table Register**|--------------------------------------------------|
|  LDTR from register|0000 1111 : 0000 0000 : 11 010 reg|
|  LDTR from memory|0000 1111 : 0000 0000 : mod 010 r/m|
|**LMSW – Load Machine Status Word**|--------------------------------------------------|
|  from register|0000 1111 : 0000 0001 : 11 110 reg|
|  from memory|0000 1111 : 0000 0001 : mod 110 r/m|
|**LOCK – Assert LOCK# Signal Prefix**|1111 0000|
|**LODS/LODSB/LODSW/LODSD – Load String Operand**|1010 110w|
|**LOOP – Loop Count**|1110 0010 : 8-bit displacement|
|**LOOPZ/LOOPE – Loop Count while Zero/Equal**|1110 0001 : 8-bit displacement|
|**LOOPNZ/LOOPNE – Loop Count while not Zero/Equal**|1110 0000 : 8-bit displacement|
|**LSL – Load Segment Limit**|--------------------------------------------------|
|  from register|0000 1111 : 0000 0011 : 11 reg1 reg2|
|  from memory|0000 1111 : 0000 0011 : mod reg r/m|
|**LSS – Load Pointer to SS**|0000 1111 : 1011 0010 : modA reg r/m|
|**LTR – Load Task Register**|--------------------------------------------------|
|  from register|0000 1111 : 0000 0000 : 11 011 reg|
|  from memory|0000 1111 : 0000 0000 : mod 011 r/m|
|**MOV – Move Data**|--------------------------------------------------|
|  register1 to register2|1000 100w : 11 reg1 reg2|
|  register2 to register1|1000 101w : 11 reg1 reg2|
|  memory to reg|1000 101w : mod reg r/m|
|  reg to memory|1000 100w : mod reg r/m|
|  immediate to register|1100 011w : 11 000 reg : immediate data|
|  immediate to register (alternate encoding)|1011 w reg : immediate data|
|  immediate to memory|1100 011w : mod 000 r/m : immediate data|
|  memory to AL, AX, or EAX|1010 000w : full displacement|
|  AL, AX, or EAX to memory|1010 001w : full displacement|
|**MOV – Move to/from Control Registers**|--------------------------------------------------|
|  CR0 from register|0000 1111 : 0010 0010 : -- 000 reg|
|  CR2 from register|0000 1111 : 0010 0010 : -- 010reg|
|  CR3 from register|0000 1111 : 0010 0010 : -- 011 reg|
|  CR4 from register|0000 1111 : 0010 0010 : -- 100 reg|
|  register from CR0-CR4|0000 1111 : 0010 0000 : -- eee reg|
|**MOV – Move to/from Debug Registers**|--------------------------------------------------|
|  DR0-DR3 from register|0000 1111 : 0010 0011 : -- eee reg|
|  DR4-DR5 from register|0000 1111 : 0010 0011 : -- eee reg|
|  DR6-DR7 from register|0000 1111 : 0010 0011 : -- eee reg|
|  register from DR6-DR7|0000 1111 : 0010 0001 : -- eee reg|
|  register from DR4-DR5|0000 1111 : 0010 0001 : -- eee reg|
|  register from DR0-DR3|0000 1111 : 0010 0001 : -- eee reg|
|**MOV – Move to/from Segment Registers**|--------------------------------------------------|
|  register to segment register|1000 1110 : 11 sreg3 reg|
|  register to SS|1000 1110 : 11 sreg3 reg|
|  memory to segment reg|1000 1110 : mod sreg3 r/m|
|  memory to SS|1000 1110 : mod sreg3 r/m|
|  segment register to register|1000 1100 : 11 sreg3 reg|
|  segment register to memory|1000 1100 : mod sreg3 r/m|
|**MOVBE – Move data after swapping bytes**|--------------------------------------------------|
|  memory to register|0000 1111 : 0011 1000:1111 0000 : mod reg r/m|
|  register to memory|0000 1111 : 0011 1000:1111 0001 : mod reg r/m|
|**MOVS/MOVSB/MOVSW/MOVSD – Move Data from String to String**|1010 010w|
|**MOVSX – Move with Sign-Extend**|--------------------------------------------------|
|  memory to reg|0000 1111 : 1011 111w : mod reg r/m|
|**MOVZX – Move with Zero-Extend**|--------------------------------------------------|
|  register2 to register1|0000 1111 : 1011 011w : 11 reg1 reg2|
|  memory to register|0000 1111 : 1011 011w : mod reg r/m|
|**MUL – Unsigned Multiply**|--------------------------------------------------|
|  AL, AX, or EAX with register|1111 011w : 11 100 reg|
|  AL, AX, or EAX with memory|1111 011w : mod 100 r/m|
|**NEG – Two's Complement Negation**|--------------------------------------------------|
|  register|1111 011w : 11 011 reg|
|  memory|1111 011w : mod 011 r/m|
|**NOP – No Operation**|1001 0000|--------------------------------------------------|
|**NOP – Multi-byte No Operation**|--------------------------------------------------|
|  register|0000 1111 0001 1111 : 11 000 reg|
|  memory|0000 1111 0001 1111 : mod 000 r/m|
|**NOT – One's Complement Negation**|--------------------------------------------------|
|  register|1111 011w : 11 010 reg|
|  memory|1111 011w : mod 010 r/m|
|**OR – Logical Inclusive OR**|--------------------------------------------------|
|  register1 to register2|0000 100w : 11 reg1 reg2|
|  register2 to register1|0000 101w : 11 reg1 reg2|
|  memory to register|0000 101w : mod reg r/m|
|  register to memory|0000 100w : mod reg r/m|
|  immediate to register|1000 00sw : 11 001 reg : immediate data|
|  immediate to AL, AX, or EAX|0000 110w : immediate data|
|  immediate to memory|1000 00sw : mod 001 r/m : immediate data|
|**OUT – Output to Port**|--------------------------------------------------|
|  fixed port|1110 011w : port number|
|  variable port|1110 111w|
|**OUTS – Output to DX Port 0110 111w**|--------------------------------------------------|
|**POP – Pop a Word from the Stack**|--------------------------------------------------|
|  register|1000 1111 : 11 000 reg|
|  register (alternate encoding)|0101 1 reg|
|  memory|1000 1111 : mod 000 r/m|
|**POP – Pop a Segment Register from the Stack (Note: CS cannot be sreg2 in this usage.)**|--------------------------------------------------|
|  segment register DS, ES|000 sreg2 111|
|  segment register SS|000 sreg2 111|
|  segment register FS, GS|0000 1111: 10 sreg3 001|
|**POPA/POPAD – Pop All General Registers**|0110 0001|
|**POPF/POPFD – Pop Stack into FLAGS or EFLAGS Register**|1001 1101|
|**PUSH – Push Operand onto the Stack**|--------------------------------------------------|
|  register|1111 1111 : 11 110 reg|
|  register (alternate encoding)|0101 0 reg|
|  memory|1111 1111 : mod 110 r/m|
|  immediate|0110 10s0 : immediate data|
|**PUSH – Push Segment Register onto the Stack**|--------------------------------------------------|
|  segment register CS,DS,ES,SS|000 sreg2 110|
|  segment register FS,GS|0000 1111: 10 sreg3 000|
|**PUSHA/PUSHAD – Push All General Registers**|0110 0000|
|**PUSHF/PUSHFD – Push Flags Register onto the Stack**|1001 1100|
|**RCL – Rotate thru Carry Left**|--------------------------------------------------|
|  register by 1|1101 000w : 11 010 reg|
|  memory by 1|1101 000w : mod 010 r/m|
|  register by CL|1101 001w : 11 010 reg|
|  memory by CL|1101 001w : mod 010 r/m|
|  register by immediate count|1100 000w : 11 010 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 010 r/m : imm8 data|
|**RCR – Rotate thru Carry Right**|--------------------------------------------------|
|  register by 1|1101 000w : 11 011 reg|
|  memory by 1|1101 000w : mod 011 r/m|
|  register by CL|1101 001w : 11 011 reg|
|  memory by CL|1101 001w : mod 011 r/m|
|  register by immediate count|1100 000w : 11 011 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 011 r/m : imm8 data|
|**RDMSR – Read from Model-Specific Register**|0000 1111 : 0011 0010|
|**RDPMC – Read Performance Monitoring Counters**|0000 1111 : 0011 0011|
|**RDTSC – Read Time-Stamp Counter**|0000 1111 : 0011 0001|
|**RDTSCP – Read Time-Stamp Counter and Processor ID**|0000 1111 : 0000 0001: 1111 1001|
|**REP INS – Input String**|1111 0011 : 0110 110w|
|**REP LODS – Load String**|1111 0011 : 1010 110w|
|**REP MOVS – Move String**|1111 0011 : 1010 010w|
|**REP OUTS – Output String**|1111 0011 : 0110 111w|
|**REP STOS – Store String**|1111 0011 : 1010 101w|
|**REPE CMPS – Compare String**|1111 0011 : 1010 011w|
|**REPE SCAS – Scan String**|1111 0011 : 1010 111w|
|**REPNE CMPS – Compare String**|1111 0010 : 1010 011w|
|**REPNE SCAS – Scan String**|1111 0010 : 1010 111w|
|**RET – Return from Procedure (to same segment)**|--------------------------------------------------|
|  no argument|1100 0011|
|  adding immediate to SP|1100 0010 : 16-bit displacement|
|**RET – Return from Procedure (to other segment)**|--------------------------------------------------|
|  intersegment|1100 1011|
|  adding immediate to SP|1100 1010 : 16-bit displacement|
|**ROL – Rotate Left**|--------------------------------------------------|
|  register by 1|1101 000w : 11 000 reg|
|  memory by 1|1101 000w : mod 000 r/m|
|  register by CL|1101 001w : 11 000 reg|
|  memory by CL|1101 001w : mod 000 r/m|
|  register by immediate count|1100 000w : 11 000 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 000 r/m : imm8 data|
|**ROR – Rotate Right**|--------------------------------------------------|
|  register by 1|1101 000w : 11 001 reg|
|  memory by 1|1101 000w : mod 001 r/m|
|  register by CL|1101 001w : 11 001 reg|
|  memory by CL|1101 001w : mod 001 r/m|
|  register by immediate count|1100 000w : 11 001 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 001 r/m : imm8 data|
|**RSM – Resume from System Management Mode**|0000 1111 : 1010 1010|
|**SAHF – Store AH into Flags**|1001 1110|
|**SAL – Shift Arithmetic Left**|same instruction as SHL|
|**SAR – Shift Arithmetic Right**|--------------------------------------------------|
|  register by 1|1101 000w : 11 111 reg|
|  memory by 1|1101 000w : mod 111 r/m|
|  register by CL|1101 001w : 11 111 reg|
|  memory by CL|1101 001w : mod 111 r/m|
|  register by immediate count|1100 000w : 11 111 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 111 r/m : imm8 data|
|**SBB – Integer Subtraction with Borrow**|--------------------------------------------------|
|  register1 to register2|0001 100w : 11 reg1 reg2|
|  register2 to register1|0001 101w : 11 reg1 reg2|
|  memory to register|0001 101w : mod reg r/m|
|  register to memory|0001 100w : mod reg r/m|
|  immediate to register|1000 00sw : 11 011 reg : immediate data|
|  immediate to AL, AX, or EAX|0001 110w : immediate data|
|  immediate to memory|1000 00sw : mod 011 r/m : immediate data|
|**SCAS/SCASB/SCASW/SCASD – Scan String**|1010 111w|
|**SETcc – Byte Set on Condition**|--------------------------------------------------|
|  register|0000 1111 : 1001 tttn : 11 000 reg|
|  memory|0000 1111 : 1001 tttn : mod 000 r/m|
|**SGDT – Store Global Descriptor Table Register**|0000 1111 : 0000 0001 : modA 000 r/m|
|**SHL – Shift Left**|--------------------------------------------------|
|  register by 1|1101 000w : 11 100 reg|
|  memory by 1|1101 000w : mod 100 r/m|
|  register by CL|1101 001w : 11 100 reg|
|  memory by CL|1101 001w : mod 100 r/m|
|  register by immediate count|1100 000w : 11 100 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 100 r/m : imm8 data|
|**SHLD – Double Precision Shift Left**|--------------------------------------------------|
|  register by immediate count|0000 1111 : 1010 0100 : 11 reg2 reg1 : imm8|
|  memory by immediate count|0000 1111 : 1010 0100 : mod reg r/m : imm8|
|  register by CL|0000 1111 : 1010 0101 : 11 reg2 reg1|
|  memory by CL|0000 1111 : 1010 0101 : mod reg r/m|
|**SHR – Shift Right**|--------------------------------------------------|
|  register by 1|1101 000w : 11 101 reg|
|  memory by 1|1101 000w : mod 101 r/m|
|  register by CL|1101 001w : 11 101 reg|
|  memory by CL|1101 001w : mod 101 r/m|
|  register by immediate count|1100 000w : 11 101 reg : imm8 data|
|  memory by immediate count|1100 000w : mod 101 r/m : imm8 data|
|**SHRD – Double Precision Shift Right**|--------------------------------------------------|
|  register by immediate count|0000 1111 : 1010 1100 : 11 reg2 reg1 : imm8|
|  memory by immediate count|0000 1111 : 1010 1100 : mod reg r/m : imm8|
|  register by CL|0000 1111 : 1010 1101 : 11 reg2 reg1|
|  memory by CL|0000 1111 : 1010 1101 : mod reg r/m|
|**SIDT – Store Interrupt Descriptor Table Register**|0000 1111 : 0000 0001 : modA 001 r/m|
|**SLDT – Store Local Descriptor Table Register**|--------------------------------------------------|
|  to register|0000 1111 : 0000 0000 : 11 000 reg|
|  to memory|0000 1111 : 0000 0000 : mod 000 r/m|
|**SMSW – Store Machine Status Word**|
|  to register|0000 1111 : 0000 0001 : 11 100 reg|
|  to memory|0000 1111 : 0000 0001 : mod 100 r/m|
|**STC – Set Carry Flag**|1111 1001|
|**STD – Set Direction Flag**|1111 1101|
|**STI – Set Interrupt Flag**|1111 1011|
|**STOS/STOSB/STOSW/STOSD – Store String Data**|1010 101w|
|**STR – Store Task Register**|--------------------------------------------------|
|  to register|0000 1111 : 0000 0000 : 11 001 reg|
|  to memory|0000 1111 : 0000 0000 : mod 001 r/m|
|**SUB – Integer Subtraction**|--------------------------------------------------|
|  register1 to register2|0010 100w : 11 reg1 reg2|
|  register2 to register1|0010 101w : 11 reg1 reg2|
|  memory to register|0010 101w : mod reg r/m|
|  register to memory|0010 100w : mod reg r/m|
|  immediate to register|1000 00sw : 11 101 reg : immediate data|
|  immediate to AL, AX, or EAX|0010 110w : immediate data|
|  immediate to memory|1000 00sw : mod 101 r/m : immediate data|
|**TEST – Logical Compare**|--------------------------------------------------|
|  register1 and register2|1000 010w : 11 reg1 reg2|
|  memory and register|1000 010w : mod reg r/m|
|  immediate and register|1111 011w : 11 000 reg : immediate data|
|  immediate and AL, AX, or EAX|1010 100w : immediate data|
|  immediate and memory|1111 011w : mod 000 r/m : immediate data|
|**UD0 – Undefined instruction**|0000 1111 : 1111 1111|
|**UD1 – Undefined instruction**|0000 1111 : 0000 1011|
|**UD2 – Undefined instruction**|0000 FFFF : 0000 1011|
|**VERR – Verify a Segment for Reading**|--------------------------------------------------|
|  register|0000 1111 : 0000 0000 : 11 100 reg|
|  memory|0000 1111 : 0000 0000 : mod 100 r/m|
|**VERW – Verify a Segment for Writing**|--------------------------------------------------|
|  register|0000 1111 : 0000 0000 : 11 101 reg|
|  memory|0000 1111 : 0000 0000 : mod 101 r/m|
|**WAIT – Wait**|1001 1011|
|**WBINVD – Writeback and Invalidate Data Cache**|0000 1111 : 0000 1001|
|**WRMSR – Write to Model-Specific Register**|0000 1111 : 0011 0000|
|**XADD – Exchange and Add**|--------------------------------------------------|
|  register1, register2|0000 1111 : 1100 000w : 11 reg2 reg1|
|  memory, reg|0000 1111 : 1100 000w : mod reg r/m|
|**XCHG – Exchange Register/Memory with Register**|--------------------------------------------------|
|  register1 with register2|1000 011w : 11 reg1 reg2|
|  AX or EAX with reg|1001 0 reg|
|  memory with reg|1000 011w : mod reg r/m|
|**XLAT/XLATB – Table Look-up Translation**|1101 0111|
|**XOR – Logical Exclusive OR**|--------------------------------------------------|
|  register1 to register2|0011 000w : 11 reg1 reg2|
|  register2 to register1|0011 001w : 11 reg1 reg2|
|  memory to register|0011 001w : mod reg r/m|
|  register to memory|0011 000w : mod reg r/m|
|  immediate to register|1000 00sw : 11 110 reg : immediate data|
|  immediate to AL, AX, or EAX|0011 010w : immediate data|
|  immediate to memory|1000 00sw : mod 110 r/m : immediate data|
|**Prefix Bytes**|--------------------------------------------------|
|  address size|0110 0111|
|  LOCK|1111 0000|
|  operand size|0110 0110|
|  CS segment override|0010 1110|
|  DS segment override|0011 1110|
|  ES segment override|0010 0110|
|  FS segment override|0110 0100|
|  GS segment override|0110 0101|
|  SS segment override|0011 0110|

Notes:

* The multi-byte NOP instruction does not alter the content of the register and will not issue a memory operation.

## General Purpose Instruction Formats and Encodings for 64-Bit Mode

Tables below show machine instruction formats and encodings for general purpose instructions in 64-bit mode.

|Symbol|Application|
|:-:|:-|
|S|If the value of REX.W. is 1, it overrides the presence of 66H.|
|W|The value of bit W. in REX is has no effect.|

|Instruction and Format|Encoding|
|:-|:-|
|**ADC – ADD with Carry**|--------------------------------------------------|
|  register1 to register2|0100 0R0B : 0001 000w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B : 0001 0001 : 11 qwordreg1 qwordreg2|
|  register2 to register1|0100 0R0B : 0001 001w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B : 0001 0011 : 11 qwordreg1 qwordreg2|
|  memory to register|0100 0RXB : 0001 001w : mod reg r/m|
|  memory to qwordregister|0100 1RXB : 0001 0011 : mod qwordreg r/m|
|  register to memory|0100 0RXB : 0001 000w : mod reg r/m|
|  qwordregister to memory|0100 1RXB : 0001 0001 : mod qwordreg r/m|
|  immediate to register|0100 000B : 1000 00sw : 11 010 reg : immediate|
|  immediate to qwordregister|0100 100B : 1000 0001 : 11 010 qwordreg : imm32|
|  immediate to qwordregister|0100 1R0B : 1000 0011 : 11 010 qwordreg : imm8|
|  immediate to AL, AX, or EAX|0001 010w : immediate data|
|  immediate to RAX|0100 1000 : 0000 0101 : imm32|
|  immediate to memory|0100 00XB : 1000 00sw : mod 010 r/m : immediate |
|  immediate32 to memory64|0100 10XB : 1000 0001 : mod 010 r/m : imm32|
|  immediate8 to memory64|0100 10XB : 1000 0031 : mod 010 r/m : imm8|
|**ADD – Add**|--------------------------------------------------|
|  register1 to register2|0100 0R0B : 0000 000w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B 0000 0000 : 11 qwordreg1 qwordreg2|
|  register2 to register1|0100 0R0B : 0000 001w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B 0000 0010 : 11 qwordreg1 qwordreg2|
|  memory to register|0100 0RXB : 0000 001w : mod reg r/m|
|  memory64 to qwordregister|0100 1RXB : 0000 0000 : mod qwordreg r/m|
|  register to memory|0100 0RXB : 0000 000w : mod reg r/m|
|  qwordregister to memory64|0100 1RXB : 0000 0011 : mod qwordreg r/m|
|  immediate to register|0100 0000B : 1000 00sw : 11 000 reg : immediate data|
|  immediate32 to qwordregister|0100 100B : 1000 0001 : 11 010 qwordreg : imm|
|  immediate to AL, AX, or EAX|0000 010w : immediate8|
|  immediate to RAX|0100 1000 : 0000 0101 : imm32|
|  immediate to memory|0100 00XB : 1000 00sw : mod 000 r/m : immediate|
|  immediate32 to memory64|0100 10XB : 1000 0001 : mod 010 r/m : imm32|
|  immediate8 to memory64|0100 10XB : 1000 0011 : mod 010 r/m : imm8|
|**AND – Logical AND**|--------------------------------------------------|
|  register1 to register2|0100 0R0B 0010 000w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B 0010 0001 : 11 qwordreg1 qwordreg2|
|  register2 to register1|0100 0R0B 0010 001w : 11 reg1 reg2 |
|  register1 to register2|0100 1R0B 0010 0011 : 11 qwordreg1 qwordreg2|
|  memory to register|0100 0RXB 0010 001w : mod reg r/m|
|  memory64 to qwordregister|0100 1RXB : 0010 0011 : mod qwordreg r/m|
|  register to memory|0100 0RXB : 0010 000w : mod reg r/m|
|  qwordregister to memory64|0100 1RXB : 0010 0001 : mod qwordreg r/m|
|  immediate to register|0100 000B : 1000 00sw : 11 100 reg : immediate |
|  immediate32 to qwordregister|0100 100B 1000 0001 : 11 100 qwordreg : imm32|
|  immediate to AL, AX, or EAX|0010 010w : immediate|
|  immediate32 to RAX|0100 1000 0010 1001 : imm32|
|  immediate to memory|0100 00XB : 1000 00sw : mod 100 r/m : immediate |
|  immediate32 to memory64|0100 10XB : 1000 0001 : mod 100 r/m : immediate32|
|  immediate8 to memory64|0100 10XB : 1000 0011 : mod 100 r/m : imm8|
|**BSF – Bit Scan Forward**|--------------------------------------------------|
|  register1, register2|0100 0R0B 0000 1111 : 1011 1100 : 11 reg1 reg2|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1011 1100 : 11 qwordreg1 qwordreg2|
|  memory, register|0100 0RXB 0000 1111 : 1011 1100 : mod reg r/m|
|  memory64, qwordregister|0100 1RXB 0000 1111 : 1011 1100 : mod qwordreg r/m|
|**BSR – Bit Scan Reverse**|--------------------------------------------------|
|  register1, register2|0100 0R0B 0000 1111 : 1011 1101 : 11 reg1 reg2|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1011 1101 : 11 qwordreg1 qwordreg2|
|  memory, register|0100 0RXB 0000 1111 : 1011 1101 : mod reg r/m|
|  memory64, qwordregister|0100 1RXB 0000 1111 : 1011 1101 : mod qwordreg r/m|
|**BSWAP – Byte Swap**|0000 1111 : 1100 1 reg|
|**BSWAP – Byte Swap**|0100 100B 0000 1111 : 1100 1 qwordreg|
|**BT – Bit Test**|--------------------------------------------------|
|  register, immediate|0100 000B 0000 1111 : 1011 1010 : 11 100 reg: imm8|
|  qwordregister, immediate8|0100 100B 1111 : 1011 1010 : 11 100 qwordreg: imm8 data|
|  memory, immediate|0100 00XB 0000 1111 : 1011 1010 : mod 100 r/m : imm8|
|  memory64, immediate8|0100 10XB 0000 1111 : 1011 1010 : mod 100 r/m : imm8 data|
|  register1, register2|0100 0R0B 0000 1111 : 1010 0011 : 11 reg2 reg1|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1010 0011 : 11 qwordreg2 qwordreg1|
|  memory, reg|0100 0RXB 0000 1111 : 1010 0011 : mod reg r/m|
|  memory, qwordreg|0100 1RXB 0000 1111 : 1010 0011 : mod qwordreg r/m|
|**BTC – Bit Test and Complemen**t|--------------------------------------------------|
|  register, immediate|0100 000B 0000 1111 : 1011 1010 : 11 111 reg: imm8|
|  qwordregister, immediate8|0100 100B 0000 1111 : 1011 1010 : 11 111 qwordreg: imm8|
|  memory, immediate|0100 00XB 0000 1111 : 1011 1010 : mod 111 r/m : imm8|
|  memory64, immediate8|0100 10XB 0000 1111 : 1011 1010 : mod 111 r/m : imm8|
|  register1, register2|0100 0R0B 0000 1111 : 1011 1011 : 11 reg2 reg1|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1011 1011 : 11 qwordreg2 qwordreg1|
|  memory, register|0100 0RXB 0000 1111 : 1011 1011 : mod reg r/m|
|  memory, qwordreg|0100 1RXB 0000 1111 : 1011 1011 : mod qwordreg r/m|
|**BTR – Bit Test and Reset**|--------------------------------------------------|
|  register, immediate|0100 000B 0000 1111 : 1011 1010 : 11 110 reg: imm8|
|  qwordregister, immediate8|0100 100B 0000 1111 : 1011 1010 : 11 110 qwordreg: imm8|
|  memory, immediate|0100 00XB 0000 1111 : 1011 1010 : mod 110 r/m : imm8|
|  memory64, immediate8|0100 10XB 0000 1111 : 1011 1010 : mod 110 r/m : imm8|
|  register1, register2|0100 0R0B 0000 1111 : 1011 0011 : 11 reg2 reg1|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1011 0011 : 11 qwordreg2 qwordreg1|
|  memory, register|0100 0RXB 0000 1111 : 1011 0011 : mod reg r/m|
|  memory64, qwordreg|0100 1RXB 0000 1111 : 1011 0011 : mod qwordreg r/m|
|**BTS – Bit Test and Set**|--------------------------------------------------|
|  register, immediate|0100 000B 0000 1111 : 1011 1010 : 11 101 reg: imm8|
|  qwordregister, immediate8|0100 100B 0000 1111 : 1011 1010 : 11 101 qwordreg: imm8|
|  memory, immediate|0100 00XB 0000 1111 : 1011 1010 : mod 101 r/m : imm8|
|  memory64, immediate8|0100 10XB 0000 1111 : 1011 1010 : mod 101 r/m : imm8|
|  register1, register2|0100 0R0B 0000 1111 : 1010 1011 : 11 reg2 reg1|
|  qwordregister1, qwordregister2|0100 1R0B 0000 1111 : 1010 1011 : 11 qwordreg2 qwordreg1|
|  memory, register|0100 0RXB 0000 1111 : 1010 1011 : mod reg r/m|
|  memory64, qwordreg|0100 1RXB 0000 1111 : 1010 1011 : mod qwordreg r/m|
|**CALL – Call Procedure (in same segment)**|--------------------------------------------------|
|  direct|1110 1000 : displacement32|
|  register indirect|0100 WR00w 1111 1111 : 11 010 reg|
|  memory indirect|0100 W0XBw 1111 1111 : mod 010 r/m|
|**CALL – Call Procedure (in other segment)**|--------------------------------------------------|
|  indirect|1111 1111 : mod 011 r/m|
|  indirect|0100 10XB 0100 1000 1111 1111 : mod 011 r/m|
|**CBW – Convert Byte to Word**|1001 1000|
|**CDQ – Convert Doubleword to Qword+**|1001 1001|
|**CDQE – RAX, Sign-Extend of EAX**|0100 1000 1001 1001|
|**CLC – Clear Carry Flag**|1111 1000|
|**CLD – Clear Direction Flag**|1111 1100|
|**CLI – Clear Interrupt Flag**|1111 1010|
|**CLTS – Clear Task-Switched Flag in CR0**|0000 1111 : 0000 0110|
|**CMC – Complement Carry Fla**g|1111 0101|
|**CMP – Compare Two Operands**|--------------------------------------------------|
|  register1 with register2|0100 0R0B 0011 100w : 11 reg1 reg2|
|  qwordregister1 with qwordregister2|0100 1R0B 0011 1001 : 11 qwordreg1 qwordreg2|
|  register2 with register1|0100 0R0B 0011 101w : 11 reg1 reg2|
|  qwordregister2 with qwordregister1|0100 1R0B 0011 101w : 11 qwordreg1 qwordreg2|
|  memory with register|0100 0RXB 0011 100w : mod reg r/m|
|  memory64 with qwordregister|0100 1RXB 0011 1001 : mod qwordreg r/m|
|  register with memory|0100 0RXB 0011 101w : mod reg r/m|
|  qwordregister with memory64|0100 1RXB 0011 101w1 : mod qwordreg r/m|
|  immediate with register|0100 000B 1000 00sw : 11 111 reg : imm|
|  immediate32 with qwordregister|0100 100B 1000 0001 : 11 111 qwordreg : imm64|
|  immediate with AL, AX, or EAX|0011 110w : imm|
|  immediate32 with RAX|0100 1000 0011 1101 : imm32|
|  immediate with memory|0100 00XB 1000 00sw : mod 111 r/m : imm|
|  immediate32 with memory64|0100 1RXB 1000 0001 : mod 111 r/m : imm64|
|  immediate8 with memory64|0100 1RXB 1000 0011 : mod 111 r/m : imm8|
|**CMPS/CMPSB/CMPSW/CMPSD/CMPSQ – Compare String Operands**|--------------------------------------------------|
|  compare string operands [ X at DS:(E)SI with Y at ES:(E)DI ]|1010 011w|
|  qword at address RSI with qword at address RDI|0100 1000 1010 0111|
|**CMPXCHG – Compare and Exchange**|--------------------------------------------------|
|  register1, register2|0000 1111 : 1011 000w : 11 reg2 reg1|
|  byteregister1, byteregister2|0100 000B 0000 1111 : 1011 0000 : 11 bytereg2 reg1|
|  qwordregister1, qwordregister2|0100 100B 0000 1111 : 1011 0001 : 11 qwordreg2 reg1|
|  memory, register|0000 1111 : 1011 000w : mod reg r/m|
|  memory8, byteregister|0100 00XB 0000 1111 : 1011 0000 : mod bytereg r/m|
|  memory64, qwordregister|0100 10XB 0000 1111 : 1011 0001 : mod qwordreg r/m|
|**CPUID – CPU Identification**|0000 1111 : 1010 0010|
|**CQO – Sign-Extend RAX**|0100 1000 1001 1001|
|**CWD – Convert Word to Doubleword**|1001 1001|
|**CWDE – Convert Word to Doubleword**|1001 1000|
|**DEC – Decrement by 1**|--------------------------------------------------|
|  register|0100 000B 1111 111w : 11 001 reg|
|  qwordregister|0100 100B 1111 1111 : 11 001 qwordreg|
|  memory|0100 00XB 1111 111w : mod 001 r/m|
|  memory64|0100 10XB 1111 1111 : mod 001 r/m|
|**DIV – Unsigned Divide**|--------------------------------------------------|
|  AL, AX, or EAX by register|0100 000B 1111 011w : 11 110 reg|
|  Divide RDX:RAX by qwordregister|0100 100B 1111 0111 : 11 110 qwordreg|
|  AL, AX, or EAX by memory|0100 00XB 1111 011w : mod 110 r/m|
|  Divide RDX:RAX by memory64|0100 10XB 1111 0111 : mod 110 r/m|
|**ENTER – Make Stack Frame for High Level Procedure**|1100 1000 : 16-bit displacement : 8-bit level (L)|
|**HLT – Halt**|1111 0100|
|**IDIV – Signed Divide**|--------------------------------------------------|
|  AL, AX, or EAX by register|0100 000B 1111 011w : 11 111 reg|
|  RDX:RAX by qwordregister|0100 100B 1111 0111 : 11 111 qwordreg|
|  AL, AX, or EAX by memory|0100 00XB 1111 011w : mod 111 r/m|
|  RDX:RAX by memory64|0100 10XB 1111 0111 : mod 111 r/m|
|**IMUL – Signed Multiply**|--------------------------------------------------|
|  AL, AX, or EAX with register|0100 000B 1111 011w : 11 101 reg|
|  RDX:RAX := RAX with qwordregister|0100 100B 1111 0111 : 11 101 qwordreg|
|  AL, AX, or EAX with memory|0100 00XB 1111 011w : mod 101 r/m|
|  RDX:RAX := RAX with memory64|0100 10XB 1111 0111 : mod 101 r/m|
|  register1 with register2|0000 1111 : 1010 1111 : 11 : reg1 reg2|
|  qwordregister1 := qwordregister1 with qwordregister2|0100 1R0B 0000 1111 : 1010 1111 : 11 : qwordreg1 qwordreg2|
|  register with memory|0100 0RXB 0000 1111 : 1010 1111 : mod reg r/m|
|  qwordregister := qwordregister with memory64|0100 1RXB 0000 1111 : 1010 1111 : mod qwordreg r/m|
|  register1 with immediate to register2|0100 0R0B 0110 10s1 : 11 reg1 reg2 : imm|
|  qwordregister1 := qwordregister2 with sign-extended immediate8|0100 1R0B 0110 1011 : 11 qwordreg1 qwordreg2 : imm8|
|  qwordregister1 := qwordregister2 with immediate32|0100 1R0B 0110 1001 : 11 qwordreg1 qwordreg2 : imm32|
|  memory with immediate to register|0100 0RXB 0110 10s1 : mod reg r/m : imm|
|  qwordregister := memory64 with sign-extended immediate8|0100 1RXB 0110 1011 : mod qwordreg r/m : imm8|
|  qwordregister := memory64 with immediate32|0100 1RXB 0110 1001 : mod qwordreg r/m : imm32|
|**IN – Input From Port**|--------------------------------------------------|
|  fixed port|1110 010w : port number|
|  variable port|1110 110w|
|**INC – Increment by 1**|--------------------------------------------------|
|  reg|0100 000B 1111 111w : 11 000 reg|
|  qwordreg|0100 100B 1111 1111 : 11 000 qwordreg|
|  memory|0100 00XB 1111 111w : mod 000 r/m|
|  memory64|0100 10XB 1111 1111 : mod 000 r/m|
|**INS – Input from DX Port**|0110 110w|
|**INT n – Interrupt Type n**|1100 1101 : type|
|**INT – Single-Step Interrupt 3**|1100 1100|
|**INTO – Interrupt 4 on Overflow**|1100 1110|
|**INVD – Invalidate Cache**|0000 1111 : 0000 1000|
|**INVLPG – Invalidate TLB Entry**|0000 1111 : 0000 0001 : mod 111 r/m|
|**INVPCID – Invalidate Process-Context Identifier**|0110 0110:0000 1111:0011 1000:1000 0010: mod reg r/m|
|**IRETO – Interrupt Return**|1100 1111|
|**Jcc – Jump if Condition is Met**|--------------------------------------------------|
|  8-bit displacement|0111 tttn : 8-bit displacement|
|  displacements (excluding 16-bit relative offsets)|0000 1111 : 1000 tttn : displacement32|
|**JCXZ/JECXZ – Jump on CX/ECX Zero**|--------------------------------------------------|
|  Address-size prefix differentiates JCXZ and JECXZ|1110 0011 : 8-bit displacement|
|**JMP – Unconditional Jump (to same segment)**|--------------------------------------------------|
|  short|1110 1011 : 8-bit displacement|
|  direct|1110 1001 : displacement32|
|  register indirect|0100 W00Bw : 1111 1111 : 11 100 reg|
|  memory indirect|0100 W0XBw : 1111 1111 : mod 100 r/m|
|**JMP – Unconditional Jump (to other segment)**|--------------------------------------------------|
|  indirect intersegment|0100 00XB : 1111 1111 : mod 101 r/m|
|  64-bit indirect intersegment|0100 10XB : 1111 1111 : mod 101 r/m|
|LAR – Load Access Rights Byte|
|  from register|0100 0R0B : 0000 1111 : 0000 0010 : 11 reg1 reg2|
|  from dwordregister to qwordregister, masked by 00FxFF00H|0100 WR0B : 0000 1111 : 0000 0010 : 11 qwordreg1 dwordreg2|
|  from memory|0100 0RXB : 0000 1111 : 0000 0010 : mod reg r/m|
|  from memory32 to qwordregister, masked by 00FxFF00H|0100 WRXB 0000 1111 : 0000 0010 : mod r/m|
|**LEA – Load Effective Address**|--------------------------------------------------|
|  in wordregister/dwordregister|0100 0RXB : 1000 1101 : modA reg r/m|
|  in qwordregister|0100 1RXB : 1000 1101 : modA qwordreg r/m|
|**LEAVE – High Level Procedure Exit**|1100 1001|
|**LFS – Load Pointer to FS**|--------------------------------------------------|
|  FS:r16/r32 with far pointer from memory|0100 0RXB : 0000 1111 : 1011 0100 : modA reg r/m|
|  FS:r64 with far pointer from memory|0100 1RXB : 0000 1111 : 1011 0100 : modA qwordreg r/m|
|**LGDT – Load Global Descriptor Table Register**|0100 10XB : 0000 1111 : 0000 0001 : modA 010 r/m|
|**LGS – Load Pointer to GS**|--------------------------------------------------|
|  GS:r16/r32 with far pointer from memory|0100 0RXB : 0000 1111 : 1011 0101 : modA reg r/m|
|  GS:r64 with far pointer from memory|0100 1RXB : 0000 1111 : 1011 0101 : modA qwordreg r/m|
|**LIDT – Load Interrupt Descriptor Table Register**|0100 10XB : 0000 1111 : 0000 0001 : modA 011 r/m|
|**LLDT – Load Local Descriptor Table Register**|--------------------------------------------------|
|  LDTR from register|0100 000B : 0000 1111 : 0000 0000 : 11 010 reg|
|  LDTR from memory|0100 00XB :0000 1111 : 0000 0000 : mod 010 r/m|
|**LMSW – Load Machine Status Word**|--------------------------------------------------|
|  from register|0100 000B : 0000 1111 : 0000 0001 : 11 110 reg|
|  from memory|0100 00XB :0000 1111 : 0000 0001 : mod 110 r/m|
|**LOCK – Assert LOCK# Signal Prefix**|1111 0000|
|**LODS/LODSB/LODSW/LODSD/LODSQ – Load String Operand**|--------------------------------------------------|
|  at DS:(E)SI to AL/EAX/EAX|1010 110w|
|  at (R)SI to RAX|0100 1000 1010 1101|
|LOOP – Loop Count|--------------------------------------------------|
|  if count ≠ 0, 8-bit displacement|1110 0010|
|  if count ≠ 0, RIP + 8-bit displacement sign-extended to 64-bits|0100 1000 1110 0010|
|**LOOPE – Loop Count while Zero/Equal**|--------------------------------------------------|
|  if count ≠ 0 & ZF =1, 8-bit displacement|1110 0001|
|  if count ≠ 0 & ZF = 1, RIP + 8-bit displacement sign-extended to 64-bits|0100 1000 1110 0001|
|**LOOPNE/LOOPNZ – Loop Count while not Zero/Equal**|--------------------------------------------------|
|  if count ≠ 0 & ZF = 0, 8-bit displacement|1110 0000|
|  if count ≠ 0 & ZF = 0, RIP + 8-bit displacement sign-extended to 64-bits|0100 1000 1110 0000|
|**LSL – Load Segment Limit**|--------------------------------------------------|
|  from register|0000 1111 : 0000 0011 : 11 reg1 reg2|
|  from qwordregister|0100 1R00 0000 1111 : 0000 0011 : 11 qwordreg1 reg2|
|  from memory16|0000 1111 : 0000 0011 : mod reg r/m|
|  from memory64|0100 1RXB 0000 1111 : 0000 0011 : mod qwordreg r/m|
|**LSS – Load Pointer to SS**|--------------------------------------------------|
|  SS:r16/r32 with far pointer from memory|0100 0RXB : 0000 1111 : 1011 0010 : modA reg r/m|
|  SS:r64 with far pointer from memory|0100 1WXB : 0000 1111 : 1011 0010 : modA qwordreg r/m|
|**LTR – Load Task Register**|--------------------------------------------------|
|  from register|0100 0R00 : 0000 1111 : 0000 0000 : 11 011 reg|
|  from memory|0100 00XB : 0000 1111 : 0000 0000 : mod 011 r/m|
|**MOV – Move Data**|--------------------------------------------------|
|  register1 to register2|0100 0R0B : 1000 100w : 11 reg1 reg2|
|  qwordregister1 to qwordregister2|0100 1R0B 1000 1001 : 11 qwordeg1 qwordreg2|
|  register2 to register1|0100 0R0B : 1000 101w : 11 reg1 reg2|
|  qwordregister2 to qwordregister1|0100 1R0B 1000 1011 : 11 qwordreg1 qwordreg2|
|  memory to reg|0100 0RXB : 1000 101w : mod reg r/m|
|  memory64 to qwordregister|0100 1RXB 1000 1011 : mod qwordreg r/m|
|  reg to memory|0100 0RXB : 1000 100w : mod reg r/m|
|  qwordregister to memory64|0100 1RXB 1000 1001 : mod qwordreg r/m|
|  immediate to register|0100 000B : 1100 011w : 11 000 reg : imm|
|  immediate32 to qwordregister (zero extend)|0100 100B 1100 0111 : 11 000 qwordreg : imm32|
|  immediate to register (alternate encoding)|0100 000B : 1011 w reg : imm|
|  immediate64 to qwordregister (alternate encoding)|0100 100B 1011 1000 reg : imm64|
|  immediate to memory|0100 00XB : 1100 011w : mod 000 r/m : imm|
|  immediate32 to memory64 (zero extend)|0100 10XB 1100 0111 : mod 000 r/m : imm32|
|  memory to AL, AX, or EAX|0100 0000 : 1010 000w : displacement|
|  memory64 to RAX|0100 1000 1010 0001 : displacement64|
|  AL, AX, or EAX to memory|0100 0000 : 1010 001w : displacement|
|  RAX to memory64|0100 1000 1010 0011 : displacement64|
|**MOV – Move to/from Control Registers**|--------------------------------------------------|
|  CR0-CR4 from register|0100 0R0B : 0000 1111 : 0010 0010 : 11 eee reg (eee = CR#)|
|  CRx from qwordregister|0100 1R0B : 0000 1111 : 0010 0010 : 11 eee qwordreg (Reee = CR#)|
|  register from CR0-CR4|0100 0R0B : 0000 1111 : 0010 0000 : 11 eee reg (eee = CR#)|
|  qwordregister from CRx|0100 1R0B 0000 1111 : 0010 0000 : 11 eee qwordreg (Reee = CR#)|
|**MOV – Move to/from Debug Registers**|--------------------------------------------------|
|  DR0-DR7 from register|0000 1111 : 0010 0011 : 11 eee reg (eee = DR#)|
|  DR0-DR7 from quadregister|0100 10OB 0000 1111 : 0010 0011 : 11 eee reg (eee = DR#)|
|  register from DR0-DR7|0000 1111 : 0010 0001 : 11 eee reg (eee = DR#)|
|  quadregister from DR0-DR7|0100 10OB 0000 1111 : 0010 0001 : 11 eee quadreg (eee = DR#)|
|**MOV – Move to/from Segment Registers**|--------------------------------------------------|
|  register to segment register|0100 W00Bw : 1000 1110 : 11 sreg reg|
|  register to SS|0100 000B : 1000 1110 : 11 sreg reg|
|  memory to segment register|0100 00XB : 1000 1110 : mod sreg r/m|
|  memory64 to segment register (lower 16 bits)|0100 10XB 1000 1110 : mod sreg r/m|
|  memory to SS|0100 00XB : 1000 1110 : mod sreg r/m|
|  segment register to register|0100 000B : 1000 1100 : 11 sreg reg|
|  segment register to qwordregister (zero extended)|0100 100B 1000 1100 : 11 sreg qwordreg|
|  segment register to memory|0100 00XB : 1000 1100 : mod sreg r/m|
|  segment register to memory64 (zero extended)|0100 10XB 1000 1100 : mod sreg3 r/m|
|**MOVBE – Move data after swapping bytes**|--------------------------------------------------|
|  memory to register|0100 0RXB : 0000 1111 : 0011 1000:1111 0000 : mod reg r/m|
|  memory64 to qwordregister|0100 1RXB : 0000 1111 : 0011 1000:1111 0000 : mod reg r/m|
|  register to memory|0100 0RXB :0000 1111 : 0011 1000:1111 0001 : mod reg r/m|
|  qwordregister to memory64|0100 1RXB :0000 1111 : 0011 1000:1111 0001 : mod reg r/m|
|**MOVS/MOVSB/MOVSW/MOVSD/MOVSQ – Move Data from String to String**|--------------------------------------------------|
|  Move data from string to string|1010 010w|
|  Move data from string to string (qword)|0100 1000 1010 0101|
|**MOVSX/MOVSXD – Move with Sign-Extend**|--------------------------------------------------|
|  register2 to register1|0100 0R0B : 0000 1111 : 1011 111w : 11 reg1 reg2|
|  byteregister2 to qwordregister1 (sign-extend)|0100 1R0B 0000 1111 : 1011 1110 : 11 quadreg1 bytereg2|
|  wordregister2 to qwordregister1|0100 1R0B 0000 1111 : 1011 1111 : 11 quadreg1 wordreg2|
|  dwordregister2 to qwordregister1|0100 1R0B 0110 0011 : 11 quadreg1 dwordreg2|
|  memory to register|0100 0RXB : 0000 1111 : 1011 111w : mod reg r/m|
|  memory8 to qwordregister (sign-extend)|0100 1RXB 0000 1111 : 1011 1110 : mod qwordreg r/m|
|  memory16 to qwordregister|0100 1RXB 0000 1111 : 1011 1111 : mod qwordreg r/m|
|  memory32 to qwordregister|0100 1RXB 0110 0011 : mod qwordreg r/m|
|**MOVZX – Move with Zero-Extend**|--------------------------------------------------|
|  register2 to register1|0100 0R0B : 0000 1111 : 1011 011w : 11 reg1 reg2|
|  dwordregister2 to qwordregister1|0100 1R0B 0000 1111 : 1011 0111 : 11 qwordreg1 dwordreg2|
|  memory to register|0100 0RXB : 0000 1111 : 1011 011w : mod reg r/m|
|  memory32 to qwordregister|0100 1RXB 0000 1111 : 1011 0111 : mod qwordreg r/m|
|**MUL – Unsigned Multiply**|--------------------------------------------------|
|  AL, AX, or EAX with register|0100 000B : 1111 011w : 11 100 reg|
|  RAX with qwordregister (to RDX:RAX)|0100 100B 1111 0111 : 11 100 qwordreg|
|  AL, AX, or EAX with memory|0100 00XB 1111 011w : mod 100 r/m|
|  RAX with memory64 (to RDX:RAX)|0100 10XB 1111 0111 : mod 100 r/m|
|**NEG – Two's Complement Negation**|--------------------------------------------------|
|  register|0100 000B : 1111 011w : 11 011 reg|
|  qwordregister|0100 100B 1111 0111 : 11 011 qwordreg|
|  memory|0100 00XB : 1111 011w : mod 011 r/m|
|  memory64|0100 10XB 1111 0111 : mod 011 r/m|
|**NOP – No Operation**|1001 0000|
|**NOT – One's Complement Negation**|--------------------------------------------------|
|  register|0100 000B : 1111 011w : 11 010 reg|
|  qwordregister|0100 000B 1111 0111 : 11 010 qwordreg|
|  memory|0100 00XB : 1111 011w : mod 010 r/m|
|  memory64|0100 1RXB 1111 0111 : mod 010 r/m|
|**OR – Logical Inclusive OR**|--------------------------------------------------|
|  register1 to register2|0000 100w : 11 reg1 reg2|
|  byteregister1 to byteregister2|0100 0R0B 0000 1000 : 11 bytereg1 bytereg2|
|  qwordregister1 to qwordregister2|0100 1R0B 0000 1001 : 11 qwordreg1 qwordreg2|
|  register2 to register1|0000 101w : 11 reg1 reg2|
|  byteregister2 to byteregister1|0100 0R0B 0000 1010 : 11 bytereg1 bytereg2|
|  qwordregister2 to qwordregister1|0100 0R0B 0000 1011 : 11 qwordreg1 qwordreg2|
|  memory to register|0000 101w : mod reg r/m|
|  memory8 to byteregister|0100 0RXB 0000 1010 : mod bytereg r/m|
|  memory8 to qwordregister|0100 0RXB 0000 1011 : mod qwordreg r/m|
|  register to memory|0000 100w : mod reg r/m|
|  byteregister to memory8|0100 0RXB 0000 1000 : mod bytereg r/m|
|  qwordregister to memory64|0100 1RXB 0000 1001 : mod qwordreg r/m|
|  immediate to register|1000 00sw : 11 001 reg : imm|
|  immediate8 to byteregister|0100 000B 1000 0000 : 11 001 bytereg : imm8|
|  immediate32 to qwordregister|0100 000B 1000 0001 : 11 001 qwordreg : imm32|
|  immediate8 to qwordregister|0100 000B 1000 0011 : 11 001 qwordreg : imm8|
|  immediate to AL, AX, or EAX|0000 110w : imm|
|  immediate64 to RAX|0100 1000 0000 1101 : imm64|
|  immediate to memory|1000 00sw : mod 001 r/m : imm|
|  immediate8 to memory8|0100 00XB 1000 0000 : mod 001 r/m : imm8|
|  immediate32 to memory64|0100 00XB 1000 0001 : mod 001 r/m : imm32|
|  immediate8 to memory64|0100 00XB 1000 0011 : mod 001 r/m : imm8|
|**OUT – Output to Port**|--------------------------------------------------|
|  fixed port|1110 011w : port number|
|  variable port|1110 111w|
|**OUTS – Output to DX Port**|--------------------------------------------------|
|  output to DX Port|0110 111w|
|**POP – Pop a Value from the Stack**|--------------------------------------------------|
|  wordregister|0101 0101 : 0100 000B : 1000 1111 : 11 000 reg16|
|  qwordregister|0100 W00BS : 1000 1111 : 11 000 reg64|
|  wordregister (alternate encoding)|0101 0101 : 0100 000B : 0101 1 reg16|
|  qwordregister (alternate encoding)|0100 W00B : 0101 1 reg64|
|  memory64|0100 W0XBS : 1000 1111 : mod 000 r/m|
|  memory16|0101 0101 : 0100 00XB 1000 1111 : mod 000 r/m|
|**POP – Pop a Segment Register from the Stack** (Note: CS cannot be sreg2 in this usage.)|--------------------------------------------------|
|  segment register FS, GS|0000 1111: 10 sreg3 001|
|**POPF/POPFQ – Pop Stack into FLAGS/RFLAGS Register**|--------------------------------------------------|
|  pop stack to FLAGS register|0101 0101 : 1001 1101|
|  pop Stack to RFLAGS register|0100 1000 : 1001 1101|
|**PUSH – Push Operand onto the Stack**|--------------------------------------------------|
|  wordregister|0101 0101 : 0100 000B : 1111 1111 : 11 110 reg16|
|  qwordregister|0100 W00BS : 1111 1111 : 11 110 reg64|
|  wordregister (alternate encoding)|0101 0101 : 0100 000B : 0101 0 reg16|
|  qwordregister (alternate encoding)|0100 W00BS : 0101 0 reg64|
|  memory16|0101 0101 : 0100 000B : 1111 1111 : mod 110 r/m|
|  memory64|0100 W00BS : 1111 1111 : mod 110 r/m|
|  immediate8|0110 1010 : imm8|
|  immediate16|0101 0101 : 0110 1000 : imm16|
|  immediate64|0110 1000 : imm64|
|**PUSH – Push Segment Register onto the Stack**|--------------------------------------------------|
|  segment register FS,GS|0000 1111: 10 sreg3 000|
|**PUSHF/PUSHFD – Push Flags Register onto the Stack**|1001 1100|
|**RCL – Rotate thru Carry Left**|--------------------------------------------------|
|  register by 1|0100 000B : 1101 000w : 11 010 reg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 010 qwordreg|
|  memory by 1|0100 00XB : 1101 000w : mod 010 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 010 r/m|
|  register by CL|0100 000B : 1101 001w : 11 010 reg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 010 qwordreg|
|  memory by CL|0100 00XB : 1101 001w : mod 010 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 010 r/m|
|  register by immediate count|0100 000B : 1100 000w : 11 010 reg : imm|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 010 qwordreg : imm8|
|  memory by immediate count|0100 00XB : 1100 000w : mod 010 r/m : imm|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 010 r/m : imm8|
|**RCR – Rotate thru Carry Right**|--------------------------------------------------|
|  register by 1|0100 000B : 1101 000w : 11 011 reg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 011 qwordreg|
|  memory by 1|0100 00XB : 1101 000w : mod 011 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 011 r/m|
|  register by CL|0100 000B : 1101 001w : 11 011 reg|
|  qwordregister by CL|0100 000B 1101 0010 : 11 011 qwordreg|
|  memory by CL|0100 00XB : 1101 001w : mod 011 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 011 r/m|
|  register by immediate count|0100 000B : 1100 000w : 11 011 reg : imm8|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 011 qwordreg : imm8|
|  memory by immediate count|0100 00XB : 1100 000w : mod 011 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 011 r/m : imm8|
|**RDMSR – Read from Model-Specific Register**|--------------------------------------------------|
|  load ECX-specified register into EDX:EAX|0000 1111 : 0011 0010|
|**RDPMC – Read Performance Monitoring Counters**|--------------------------------------------------|
|  load ECX-specified performance counter into EDX:EAX|0000 1111 : 0011 0011|
|**RDTSC – Read Time-Stamp Counter**|--------------------------------------------------|
|  read time-stamp counter into EDX:EAX|0000 1111 : 0011 0001|
|**RDTSCP – Read Time-Stamp Counter and Processor ID**|0000 1111 : 0000 0001: 1111 1001|
|**REP INS – Input String**|--------------------------------------------------|
|**REP LODS – Load String**|--------------------------------------------------|
|**REP MOVS – Move String**|--------------------------------------------------|
|**REP OUTS – Output String**|--------------------------------------------------|
|**REP STOS – Store String**|--------------------------------------------------|
|**REPE CMPS – Compare String**|--------------------------------------------------|
|**REPE SCAS – Scan String**|--------------------------------------------------|
|**REPNE CMPS – Compare String**|--------------------------------------------------|
|**REPNE SCAS – Scan String**|--------------------------------------------------|
|**RET – Return from Procedure (to same segment)**|--------------------------------------------------|
|  no argument|1100 0011|
|  adding immediate to SP|1100 0010 : 16-bit displacement|
|**RET – Return from Procedure (to other segment)**|--------------------------------------------------|
|  intersegment|1100 1011|
|  adding immediate to SP|1100 1010 : 16-bit displacement|
|**ROL – Rotate Left**|--------------------------------------------------|
|  register by 1|0100 000B 1101 000w : 11 000 reg|
|  byteregister by 1|0100 000B 1101 0000 : 11 000 bytereg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 000 qwordreg|
|  memory by 1|0100 00XB 1101 000w : mod 000 r/m|
|  memory8 by 1|0100 00XB 1101 0000 : mod 000 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 000 r/m|
|  register by CL|0100 000B 1101 001w : 11 000 reg|
|  byteregister by CL|0100 000B 1101 0010 : 11 000 bytereg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 000 qwordreg|
|  memory by CL|0100 00XB 1101 001w : mod 000 r/m|
|  memory8 by CL|0100 00XB 1101 0010 : mod 000 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 000 r/m|
|  register by immediate count|1100 000w : 11 000 reg : imm8|
|  byteregister by immediate count|0100 000B 1100 0000 : 11 000 bytereg : imm8|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 000 bytereg : imm8|
|  memory by immediate count|1100 000w : mod 000 r/m : imm8|
|  memory8 by immediate count|0100 00XB 1100 0000 : mod 000 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 000 r/m : imm8|
|**ROR – Rotate Right**|--------------------------------------------------|
|  register by 1|0100 000B 1101 000w : 11 001 reg|
|  byteregister by 1|0100 000B 1101 0000 : 11 001 bytereg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 001 qwordreg|
|  memory by 1|0100 00XB 1101 000w : mod 001 r/m|
|  memory8 by 1|0100 00XB 1101 0000 : mod 001 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 001 r/m|
|  register by CL|0100 000B 1101 001w : 11 001 reg|
|  byteregister by CL|0100 000B 1101 0010 : 11 001 bytereg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 001 qwordreg|
|  memory by CL|0100 00XB 1101 001w : mod 001 r/m|
|  memory8 by CL|0100 00XB 1101 0010 : mod 001 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 001 r/m|
|  register by immediate count|0100 000B 1100 000w : 11 001 reg : imm8 |
|  byteregister by immediate count|0100 000B 1100 0000 : 11 001 reg : imm8|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 001 qwordreg : imm8|
|  memory by immediate count|0100 00XB 1100 000w : mod 001 r/m : imm8|
|  memory8 by immediate count|0100 00XB 1100 0000 : mod 001 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 001 r/m : imm8|
|**RSM – Resume from System Management Mode**|0000 1111 : 1010 1010|
|**SAL – Shift Arithmetic Left**|same instruction as SHL|
|**SAR – Shift Arithmetic Right**|--------------------------------------------------|
|  register by 1|0100 000B 1101 000w : 11 111 reg|
|  byteregister by 1|0100 000B 1101 0000 : 11 111 bytereg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 111 qwordreg|
|  memory by 1|0100 00XB 1101 000w : mod 111 r/m|
|  memory8 by 1|0100 00XB 1101 0000 : mod 111 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 111 r/m|
|  register by CL|0100 000B 1101 001w : 11 111 reg|
|  byteregister by CL|0100 000B 1101 0010 : 11 111 bytereg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 111 qwordreg|
|  memory by CL|0100 00XB 1101 001w : mod 111 r/m|
|  memory8 by CL|0100 00XB 1101 0010 : mod 111 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 111 r/m|
|  register by immediate count|0100 000B 1100 000w : 11 111 reg : imm8|
|  byteregister by immediate count|0100 000B 1100 0000 : 11 111 bytereg : imm8|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 111 qwordreg : imm8|
|  memory by immediate count|0100 00XB 1100 000w : mod 111 r/m : imm8|
|  memory8 by immediate count|0100 00XB 1100 0000 : mod 111 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 111 r/m : imm8|
|**SBB – Integer Subtraction with Borrow**|--------------------------------------------------|
|  register1 to register2|0100 0R0B 0001 100w : 11 reg1 reg2|
|  byteregister1 to byteregister2|0100 0R0B 0001 1000 : 11 bytereg1 bytereg2|
|  quadregister1 to quadregister2|0100 1R0B 0001 1001 : 11 quadreg1 quadreg2|
|  register2 to register1|0100 0R0B 0001 101w : 11 reg1 reg2|
|  byteregister2 to byteregister1|0100 0R0B 0001 1010 : 11 reg1 bytereg2|
|  byteregister2 to byteregister1|0100 1R0B 0001 1011 : 11 reg1 bytereg2|
|  memory to register|0100 0RXB 0001 101w : mod reg r/m|
|  memory8 to byteregister|0100 0RXB 0001 1010 : mod bytereg r/m|
|  memory64 to byteregister|0100 1RXB 0001 1011 : mod quadreg r/m|
|  register to memory|0100 0RXB 0001 100w : mod reg r/m|
|  byteregister to memory8|0100 0RXB 0001 1000 : mod reg r/m|
|  quadregister to memory64|0100 1RXB 0001 1001 : mod reg r/m|
|  immediate to register|0100 000B 1000 00sw : 11 011 reg : imm|
|  immediate8 to byteregister|0100 000B 1000 0000 : 11 011 bytereg : imm8|
|  immediate32 to qwordregister|0100 100B 1000 0001 : 11 011 qwordreg : imm32|
|  immediate8 to qwordregister|0100 100B 1000 0011 : 11 011 qwordreg : imm8|
|  immediate to AL, AX, or EAX|0100 000B 0001 110w : imm|
|  immediate32 to RAL|0100 1000 0001 1101 : imm32|
|  immediate to memory|0100 00XB 1000 00sw : mod 011 r/m : imm|
|  immediate8 to memory8|0100 00XB 1000 0000 : mod 011 r/m : imm8|
|  immediate32 to memory64|0100 10XB 1000 0001 : mod 011 r/m : imm32|
|  immediate8 to memory64|0100 10XB 1000 0011 : mod 011 r/m : imm8|
|**SCAS/SCASB/SCASW/SCASD – Scan String**|--------------------------------------------------|
|  scan string|1010 111w|
|  scan string (compare AL with byte at RDI)|0100 1000 1010 1110|
|  scan string (compare RAX with qword at RDI)|0100 1000 1010 1111|
|**SETcc – Byte Set on Condition**|--------------------------------------------------|
|  register|0100 000B 0000 1111 : 1001 tttn : 11 000 reg|
|  register|0100 0000 0000 1111 : 1001 tttn : 11 000 reg|
|  memory|0100 00XB 0000 1111 : 1001 tttn : mod 000 r/m|
|  memory|0100 0000 0000 1111 : 1001 tttn : mod 000 r/m|
|**SGDT – Store Global Descriptor Table Register**|0000 1111 : 0000 0001 : modA 000 r/m|
|**SHL – Shift Left**|--------------------------------------------------|
|  register by 1|0100 000B 1101 000w : 11 100 reg|
|  byteregister by 1|0100 000B 1101 0000 : 11 100 bytereg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 100 qwordreg|
|  memory by 1|0100 00XB 1101 000w : mod 100 r/m|
|  memory8 by 1|0100 00XB 1101 0000 : mod 100 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 100 r/m|
|  register by CL|0100 000B 1101 001w : 11 100 reg|
|  byteregister by CL|0100 000B 1101 0010 : 11 100 bytereg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 100 qwordreg|
|  memory by CL|0100 00XB 1101 001w : mod 100 r/m|
|  memory8 by CL|0100 00XB 1101 0010 : mod 100 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 100 r/m|
|  register by immediate count|0100 000B 1100 000w : 11 100 reg : imm8|
|  byteregister by immediate count|0100 000B 1100 0000 : 11 100 bytereg : imm8|
|  quadregister by immediate count|0100 100B 1100 0001 : 11 100 quadreg : imm8|
|  memory by immediate count|0100 00XB 1100 000w : mod 100 r/m : imm8|
|  memory8 by immediate count|0100 00XB 1100 0000 : mod 100 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 100 r/m : imm8|
|**SHLD – Double Precision Shift Left**|--------------------------------------------------|
|  register by immediate count|0100 0R0B 0000 1111 : 1010 0100 : 11 reg2 reg1 : imm8|
|  qwordregister by immediate8|0100 1R0B 0000 1111 : 1010 0100 : 11 qworddreg2 qwordreg1 : imm8|
|  memory by immediate count|0100 0RXB 0000 1111 : 1010 0100 : mod reg r/m : imm8|
|  memory64 by immediate8|0100 1RXB 0000 1111 : 1010 0100 : mod qwordreg r/m : imm8|
|  register by CL|0100 0R0B 0000 1111 : 1010 0101 : 11 reg2 reg1|
|  quadregister by CL|0100 1R0B 0000 1111 : 1010 0101 : 11 quadreg2 quadreg1|
|  memory by CL|0100 00XB 0000 1111 : 1010 0101 : mod reg r/m|
|  memory64 by CL|0100 1RXB 0000 1111 : 1010 0101 : mod quadreg r/m|
|**SHR – Shift Right**|--------------------------------------------------|
|  register by 1|0100 000B 1101 000w : 11 101 reg|
|  byteregister by 1|0100 000B 1101 0000 : 11 101 bytereg|
|  qwordregister by 1|0100 100B 1101 0001 : 11 101 qwordreg|
|  memory by 1|0100 00XB 1101 000w : mod 101 r/m|
|  memory8 by 1|0100 00XB 1101 0000 : mod 101 r/m|
|  memory64 by 1|0100 10XB 1101 0001 : mod 101 r/m|
|  register by CL|0100 000B 1101 001w : 11 101 reg|
|  byteregister by CL|0100 000B 1101 0010 : 11 101 bytereg|
|  qwordregister by CL|0100 100B 1101 0011 : 11 101 qwordreg|
|  memory by CL|0100 00XB 1101 001w : mod 101 r/m|
|  memory8 by CL|0100 00XB 1101 0010 : mod 101 r/m|
|  memory64 by CL|0100 10XB 1101 0011 : mod 101 r/m|
|  register by immediate count|0100 000B 1100 000w : 11 101 reg : imm8|
|  byteregister by immediate count|0100 000B 1100 0000 : 11 101 reg : imm8|
|  qwordregister by immediate count|0100 100B 1100 0001 : 11 101 reg : imm8|
|  memory by immediate count|0100 00XB 1100 000w : mod 101 r/m : imm8|
|  memory8 by immediate count|0100 00XB 1100 0000 : mod 101 r/m : imm8|
|  memory64 by immediate count|0100 10XB 1100 0001 : mod 101 r/m : imm8|
|**SHRD – Double Precision Shift Right**|--------------------------------------------------|
|  register by immediate count|0100 0R0B 0000 1111 : 1010 1100 : 11 reg2 reg1 : imm8|
|  qwordregister by immediate8|0100 1R0B 0000 1111 : 1010 1100 : 11 qwordreg2 qwordreg1 : imm8|
|  memory by immediate count|0100 00XB 0000 1111 : 1010 1100 : mod reg r/m : imm8|
|  memory64 by immediate8|0100 1RXB 0000 1111 : 1010 1100 : mod qwordreg r/m : imm8|
|  register by CL|0100 000B 0000 1111 : 1010 1101 : 11 reg2 reg1|
|  qwordregister by CL|0100 1R0B 0000 1111 : 1010 1101 : 11 qwordreg2 qwordreg1|
|  memory by CL|0000 1111 : 1010 1101 : mod reg r/m|
|  memory64 by CL|0100 1RXB 0000 1111 : 1010 1101 : mod qwordreg r/m|
|**SIDT – Store Interrupt Descriptor Table Register**|0000 1111 : 0000 0001 : modA 001 r/m|
|**SLDT – Store Local Descriptor Table Register**|--------------------------------------------------|
|  to register|0100 000B 0000 1111 : 0000 0000 : 11 000 reg|
|  to memory|0100 00XB 0000 1111 : 0000 0000 : mod 000 r/m|
|**SMSW – Store Machine Status Word**|--------------------------------------------------|
|  to register|0100 000B 0000 1111 : 0000 0001 : 11 100 reg|
|  to memory|0100 00XB 0000 1111 : 0000 0001 : mod 100 r/m|
|**STC – Set Carry Flag**|1111 1001|
|**STD – Set Direction Flag**|1111 1101|
|**STI – Set Interrupt Flag**|1111 1011|
|**STOS/STOSB/STOSW/STOSD/STOSQ – Store String Data**|--------------------------------------------------|
|  store string data|1010 101w|
|  store string data (RAX at address RDI)|0100 1000 1010 1011|
|**STR – Store Task Register**|--------------------------------------------------|
|  to register|0100 000B 0000 1111 : 0000 0000 : 11 001 reg|
|  to memory|0100 00XB 0000 1111 : 0000 0000 : mod 001 r/m|
|**SUB – Integer Subtraction**|--------------------------------------------------|
|  register1 from register2|0100 0R0B 0010 100w : 11 reg1 reg2|
|  byteregister1 from byteregister2|0100 0R0B 0010 1000 : 11 bytereg1 bytereg2|
|  qwordregister1 from qwordregister2|0100 1R0B 0010 1000 : 11 qwordreg1 qwordreg2|
|  register2 from register1|0100 0R0B 0010 101w : 11 reg1 reg2|
|  byteregister2 from byteregister1|0100 0R0B 0010 1010 : 11 bytereg1 bytereg2|
|  qwordregister2 from qwordregister1|0100 1R0B 0010 1011 : 11 qwordreg1 qwordreg2|
|  memory from register|0100 00XB 0010 101w : mod reg r/m|
|  memory8 from byteregister|0100 0RXB 0010 1010 : mod bytereg r/m|
|  memory64 from qwordregister|0100 1RXB 0010 1011 : mod qwordreg r/m|
|  register from memory|0100 0RXB 0010 100w : mod reg r/m|
|  byteregister from memory8|0100 0RXB 0010 1000 : mod bytereg r/m|
|  qwordregister from memory8|0100 1RXB 0010 1000 : mod qwordreg r/m|
|  immediate from register|0100 000B 1000 00sw : 11 101 reg : imm|
|  immediate8 from byteregister|0100 000B 1000 0000 : 11 101 bytereg : imm8|
|  immediate32 from qwordregister|0100 100B 1000 0001 : 11 101 qwordreg : imm32|
|  immediate8 from qwordregister|0100 100B 1000 0011 : 11 101 qwordreg : imm8|
|  immediate from AL, AX, or EAX|0100 000B 0010 110w : imm|
|  immediate32 from RAX|0100 1000 0010 1101 : imm32|
|  immediate from memory|0100 00XB 1000 00sw : mod 101 r/m : imm|
|  immediate8 from memory8|0100 00XB 1000 0000 : mod 101 r/m : imm8|
|  immediate32 from memory64|0100 10XB 1000 0001 : mod 101 r/m : imm32|
|  immediate8 from memory64|0100 10XB 1000 0011 : mod 101 r/m : imm8|
|**SWAPGS – Swap GS Base Register**|--------------------------------------------------|
|  Exchanges the current GS base register value for value in MSR C0000102H|0000 1111 0000 0001 1111 1000|
|**SYSCALL – Fast System Call**|--------------------------------------------------|
|  fast call to privilege level 0 system procedures|0000 1111 0000 0101|
|**SYSRET – Return From Fast System Call**|--------------------------------------------------|
|  return from fast system call|0000 1111 0000 0111|
|**TEST – Logical Compare**|--------------------------------------------------|
|  register1 and register2|0100 0R0B 1000 010w : 11 reg1 reg2|
|  byteregister1 and byteregister2|0100 0R0B 1000 0100 : 11 bytereg1 bytereg2|
|  qwordregister1 and qwordregister2|0100 1R0B 1000 0101 : 11 qwordreg1 qwordreg2|
|  memory and register|0100 0R0B 1000 010w : mod reg r/m|
|  memory8 and byteregister|0100 0RXB 1000 0100 : mod bytereg r/m|
|  memory64 and qwordregister|0100 1RXB 1000 0101 : mod qwordreg r/m|
|  immediate and register|0100 000B 1111 011w : 11 000 reg : imm|
|  immediate8 and byteregister|0100 000B 1111 0110 : 11 000 bytereg : imm8|
|  immediate32 and qwordregister|0100 100B 1111 0111 : 11 000 bytereg : imm8|
|  immediate and AL, AX, or EAX|0100 000B 1010 100w : imm|
|  immediate32 and RAX|0100 1000 1010 1001 : imm32|
|  immediate and memory|0100 00XB 1111 011w : mod 000 r/m : imm|
|  immediate8 and memory8|0100 1000 1111 0110 : mod 000 r/m : imm8|
|  immediate32 and memory64|0100 1000 1111 0111 : mod 000 r/m : imm32|
|**UD2 – Undefined instruction**|0000 FFFF : 0000 1011|
|**VERR – Verify a Segment for Reading**|--------------------------------------------------|
|  register|0100 000B 0000 1111 : 0000 0000 : 11 100 reg|
|  memory|0100 00XB 0000 1111 : 0000 0000 : mod 100 r/m|
|**VERW – Verify a Segment for Writing**|--------------------------------------------------|
|  register|0100 000B 0000 1111 : 0000 0000 : 11 101 reg|
|  memory|0100 00XB 0000 1111 : 0000 0000 : mod 101 r/m|
|**WAIT – Wait**|1001 1011|
|**WBINVD – Writeback and Invalidate Data Cach**e|0000 1111 : 0000 1001|
|**WRMSR – Write to Model-Specific Register**|--------------------------------------------------|
|  write EDX:EAX to ECX specified MSR|0000 1111 : 0011 0000|
|  write RDX[31:0]:RAX[31:0] to RCX specified MSR|0100 1000 0000 1111 : 0011 0000|
|**XADD – Exchange and Add**|--------------------------------------------------|
|  register1, register2|0100 0R0B 0000 1111 : 1100 000w : 11 reg2 reg1|
|  byteregister1, byteregister2|0100 0R0B 0000 1111 : 1100 0000 : 11 bytereg2 bytereg1|
|  qwordregister1, qwordregister2|0100 0R0B 0000 1111 : 1100 0001 : 11 qwordreg2 qwordreg1|
|  memory, register|0100 0RXB 0000 1111 : 1100 000w : mod reg r/m|
|  memory8, bytereg|0100 1RXB 0000 1111 : 1100 0000 : mod bytereg r/m|
|  memory64, qwordreg|0100 1RXB 0000 1111 : 1100 0001 : mod qwordreg r/m|
|**XCHG – Exchange Register/Memory with Register**|--------------------------------------------------|
|  register1 with register2|1000 011w : 11 reg1 reg2|
|  AX or EAX with register|1001 0 reg|
|  memory with register|1000 011w : mod reg r/m|
|**XLAT/XLATB – Table Look-up Translation**|--------------------------------------------------|
|  AL to byte DS:[(E)BX + unsigned AL]|1101 0111|
|  AL to byte DS:[RBX + unsigned AL]|0100 1000 1101 0111|
|**XOR – Logical Exclusive OR**|--------------------------------------------------|
|  register1 to register2|0100 0RXB 0011 000w : 11 reg1 reg2|
|  byteregister1 to byteregister2|0100 0R0B 0011 0000 : 11 bytereg1 bytereg2|
|  qwordregister1 to qwordregister2|0100 1R0B 0011 0001 : 11 qwordreg1 qwordreg2|
|  register2 to register1|0100 0R0B 0011 001w : 11 reg1 reg2|
|  byteregister2 to byteregister1|0100 0R0B 0011 0010 : 11 bytereg1 bytereg2|
|  qwordregister2 to qwordregister1|0100 1R0B 0011 0011 : 11 qwordreg1 qwordreg2|
|  memory to register|0100 0RXB 0011 001w : mod reg r/m|
|  memory8 to byteregister|0100 0RXB 0011 0010 : mod bytereg r/m|
|  memory64 to qwordregister|0100 1RXB 0011 0011 : mod qwordreg r/m|
|  register to memory|0100 0RXB 0011 000w : mod reg r/m|
|  byteregister to memory8|0100 0RXB 0011 0000 : mod bytereg r/m|
|  qwordregister to memory8|0100 1RXB 0011 0001 : mod qwordreg r/m|
|  immediate to register|0100 000B 1000 00sw : 11 110 reg : imm|
|  immediate8 to byteregister|0100 000B 1000 0000 : 11 110 bytereg : imm8|
|  immediate32 to qwordregister|0100 100B 1000 0001 : 11 110 qwordreg : imm32|
|  immediate8 to qwordregister|0100 100B 1000 0011 : 11 110 qwordreg : imm8|
|  immediate to AL, AX, or EAX|0100 000B 0011 010w : imm|
|  immediate to RAX|0100 1000 0011 0101 : immediate data|
|  immediate to memory|0100 00XB 1000 00sw : mod 110 r/m : imm|
|  immediate8 to memory8|0100 00XB 1000 0000 : mod 110 r/m : imm8|
|  immediate32 to memory64|0100 10XB 1000 0001 : mod 110 r/m : imm32|
|  immediate8 to memory64|0100 10XB 1000 0011 : mod 110 r/m : imm8|
|**Prefix Bytes**|--------------------------------------------------|
|  address size|0110 0111|
|  LOCK|1111 0000|
|  operand size|0110 0110|
|  CS segment override|0010 1110|
|  DS segment override|0011 1110|
|  ES segment override|0010 0110|
|  FS segment override|0110 0100|
|  GS segment override|0110 0101|
|  SS segment override|0011 0110|

## Warning (Not FBI)

* This summary is for reference only and does not guarantee complete accuracy.