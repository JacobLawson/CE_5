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

The screenshot above shows the instructions for the 44 and -37 taking place. You can see that the values are loaded into registers as hex values. Then, the next few cycles show the result of the addition, 7, being stored into a register. The last cycle of interest shows this value being replaced by the 54.

For the third task, we were supposed to augment the MIPS architecture in order for it to support an ORI instruction. To begin this task, I first filled out the rest of the table in the lab handout to help create a proper control instruction and the help me think about what I wanted to do with the code. Notable things are setting the ALUop11 to OR.

After having the opcode/control code set, the next part of my design process was to make changes to the actual hardware in order to accommodate the changes. I'll be honest, I had no idea where to start, but I heard Captain Silva mention zero extender in class, so I went ahead and pursued that option in a blind leap of faith. So I made the zero extender first and foremost, afterwards I knew I would need a way to select between the sign and zero extender, so I made a multiplexer that would only pick the zero extender in the case of an ORI instruction. To accomplish this I looked at the control signal and realize that but having a specialized AND gate I could make the control signal for the multiplexer only pick the zero extend signal in the case of an ORI. The diagram below kind of communicates it better than words. 


![alt text] (http://i62.tinypic.com/oforww.png)
![alt text] (http://i62.tinypic.com/hu20id.png)
![alt text] (http://i61.tinypic.com/2urxb4h.png)
