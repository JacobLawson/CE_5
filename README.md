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

For the third task, we were supposed to augment the MIPS architecture in order for it to support an ORI instruction. To begin this task, I first filled out the rest of the table in the lab handout (shown below) to help create a proper control instruction and the help me think about what I wanted to do with the code. Notable things are setting the ALUop11 assignment to ORI.

![alt text] (http://i62.tinypic.com/oforww.png)

After having the opcode/control code set, the next part of my design process was to make changes to the actual hardware in order to accommodate the changes. I'll be honest, I had no idea where to start, but I heard Captain Silva mention zero extender in class, so I went ahead and pursued that option in a blind leap of faith. So I made the zero extender first and foremost, afterwards I knew I would need a way to select between the sign and zero extender, so I made a multiplexer that would only pick the zero extender in the case of an ORI instruction. To accomplish this I looked at the control signal and realize that but having a specialized AND gate I could make the control signal for the multiplexer only pick the zero extend signal in the case of an ORI. The diagram below kind of communicates it better than words. I am also garbage at drawing straightlines, so I made this diagram in MS paint to save your eyes.

![alt text] (http://i62.tinypic.com/hu20id.png)

The next part involved editing the actual VHDL file to implement the hardware changes. I started by looking up the sign extender code, and I made a copy of each instance while replacing the name of the copy with Zeroext. The hope was that this would replicate the device structure while the new device would be a new entity/compenent/architecture. This worked in almost every case since the devices were very similar, but the architecture behavior of the Signext had to be changed from   "y <= X"0000" & a when a(15) = '0' else X"ffff" & a;" to just y<= X"0000" for all cases. This in effect made the zeroext device complete. 

The next change that needed to occur was to create the multiplexer, the and gate, and to declare a new signal, orisig, which would serve as the mux control signal. The mux creation was a matter of looking at previous muxes and taking the template from those and using it to create a new one. I just had to change the inputs/outputs/control signal to make this work. Afterwards I needed to simple declare orisig in the architecture signal section, then I set orisig equal to ...

  orisig <= (not alucontrol(2)) and (not alucontrol(1)) and (alucontrol(0));
  
This in effect made sure that orisig would only be 1, and only select the zero extend in the case of an ORI or OR j instruction. Initially I tried using bits of the control or ALUop signal, but this did not work and I could never figure out why. My best guess is because it was not declared in the architecture of the datapath. Going off of this,I decided to use the ALU control signal since it was declared in the datapath.

Other changes worth noting were updating the case statement to recognize an ORI instruction and updating the ALUop table to recognize an ORI instruction.

After all of the above changes were made, I decided to run the testbench to see if the code worked as planned.The instructions I ran can be seen below.

