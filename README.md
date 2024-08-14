# Tiny 8 Bit CPU

## SPEC

8 bit CPU, 8 resgiter files, 8 bit memory address, 8x2 instructions

|Name|Description|Name|Description|
|:-:|:-|:-:|:-|
|x0|Constant 0|x4|Func arg|
|x1|Return address|x5|Func arg|
|x2|Stack pointer|x6|Func ret|
|x3|Temp reg|x7|Saved reg|
||
|PC|Program Pointer|
|rdrs|RD and RS addresses|


## Define assembly instructions

|Opcode|Instruction|Description|Special useage|
|:-:|:-|:-|:-|
|-0000|add rd, rs, imm|rd = rd + rs + imm||
|-0001|sub rd, rs, imm|rd = rd - rs - imm||
|0010|X|
|-0011|addu rd, rs, imm|rd = rd + rs + (imm << 4)||
||
|-0100|logic rd, rs, f|(f == 0) => rd = rd and rs<br>(f == 1) => rd = rd or rs<br>(f == 2) => rd = rd xor rs<br>(f == 3) => rd = rd << 1<br>(f == 4) => rd = rd >> 1<br>(f == 5) => rd = rd >>> 1||
|0101|X|
|0110|X|
||
|-1000|cmp rd, rs, imm|(rd < rs + imm) => rd = -1<br>(rd == rs + imm) => rd = 0<br>(rd < rs + imm) => rd = 1||
||
|-1001|jeq rd, rs, imm|(rd == rs) => PC = PC + imm|{cmp rd, rs<br>xor tmp, tmp<br>addi tmp, -1<br>jeq rd, tmp} => jump if (rd < rs)|
||
|-1010|ld rd, rs, imm|rd = mem[rs + imm]||
|-1011|st rd, rs, imm|mem[rs + imm] = rd||
||
|1100|X|
||
|1101|stpc rd, imm|rd = PC + imm|{stpc x1, 2<br>jeq x0, x0, 4 // alway jump<br>(1)... continue after function<br>(2)...<br>(3)...<br>(4)... a function here<br>...<br>ldpc x1, 0 // return}|
|-1110|ldpc rd, imm|PC = rd + imm|
||
|-0111|ldrdrs, value|rdrs = value|
|1111|X|