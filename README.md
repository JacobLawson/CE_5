CE_5
====
 This is the readme. Exciting right?
 
 For the First task, the objective was...
 
Write a MIPS assembly program that will initialize registers $S0 and $S1 with the decimal values 44 and -37 respectively (Code Example 6.10 pretty much gives the problem away). Use a register instruction to add these values together and store the result in $S2. Finally you will write an instruction that will store $S2 to the hexadecimal memory address x54.



addi $s0, $0, 44
addi $s1, $0, -37
add $s2, $s1, $s0
sw $s2, 0x54($0)

 For the second task, the objective was to...
 1) Hand assemble the above into machine code
 2) Test program using MIPs
 3) Grab a screenshot of your waveform and upload it to GitHub
 4) Explain whether the program worked and why
 

| Assembly        | Bits           | Machine Code  |
| ------------- |:-------------:| -----:|
| addi $s0, $0, 44  | 001000 00000 10000 0000000000101100   | 0x2010002C |
| addi $s1, $0, -37 | 001000 00000 10001 1111111111011011   | 0x2011FFDB |
| add $s2, $s1, $s0| 000000 10000 10001 10010 00000 100000 | 0x02119020 |
| sw $s2, 0x54($0)  | 101011 00000 10010 0000000000110110   | 0xAC120054 |


![alt text] (http://i57.tinypic.com/2gxlcn5.png)

