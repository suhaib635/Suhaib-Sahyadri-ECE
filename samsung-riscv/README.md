<h1>samsung-riscv</h1>
<h2>Basic Details</h2>
<b>Name:</b> Akshaykumar Tallur<br>
<b>College:</b> Dayananda Sagar College Of Engineering<br>
<b>Email:</b> <a href="mailto:tallurakshaykumar@gmail.com">tallurakshaykumar@gmail.com</a><br>
<b>GitHub Profile:</b> <a href="https://github.com/akshaykumartallur">akshaykumartallur</a><hr>
<!-- Task 1 -->				  
<details><p><summary><b>Task 1:</b> Task is to install RISC-V toolchain using VDI link provided,Compiling the C code and Using RISCV to get the corresponding assembly instructions for O1 and Ofast.</summary></p>
<h3>1. Install Ubuntu 18.04 LTS on Oracle Virtual Machine Box and open VDI file provided</h3><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/VM_box.png"  alt=Virtual     Machine><br><br>
<h3>2. Compiling C code</h3><br><br>
<pre><code>cd
gedit sum1ton.c
gcc sum1ton.c
./a.out</code></pre>
	
```c
#include<stdio.h>
int main(){
		int i, sum=0, n=1000;
			for (i=1;i<=n;++i){
				sum+=i;	}
		printf("Sum of Numbers from 1 to %d is %d\n",n,sum);
return 0;
	}
```

<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/C_code.png"  alt=C code><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/output_of_c_code.png"      alt=commands for c compilation><br><br>
<h3>3. Object Dump and O1, Ofast Output</h3><br><br>
<pre><code>
    cat sum1ton.c
    riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
    ls -ltr sum1ton.o
</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/assembly_commands.png"    alt=Commands ><br><br>
<pre><code>riscv64-unknown-elf-objdump -d sum1ton.o |less</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/objdump.png" alt=Object dump><br><br>
<b> For O1: The number of instructions were 15</b><br><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/O1_output.png" alt=O1 output><br><br>
<b>For Ofast: the number of instructions were 12</b><br><br>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c</code></pre><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%201/Ofast_output.png"  alt=Ofast output><br><br></details><hr>  
<!--End of Task 1-->
<!-- Task 2 -->
<!-- Spike for Sum1ton -->				
<details><p><summary>
<b>Task 2:</b> Running the SPIKE Simulation and observe the performance under the -O1 and -Ofast Compiler optimization flags.
</summary></p><details>
<p><summary>1. Sum of Integers from 1 to n</summary></p>
<h3>Debugging sum1ton.o for O1</h3>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
ls -ltr sum1ton.o
spike pk sum1ton.o
spike -d pk sum1ton.o</code></pre>
<b>O1 assembly output</b>
	
```assembly
0000000000010184 <main>:
   10184:       ff010113                addi    sp,sp,-16
   10188:       00113423                sd      ra,8(sp)
   1018c:       3e800793                li      a5,1000
   10190:       fff7879b                addiw   a5,a5,-1
   10194:       fe079ee3                bnez    a5,10190 &ltmain+0xc&gt
   10198:       0007a637                lui     a2,0x7a
   1019c:       31460613                addi    a2,a2,788 # 7a314 <__BSS_END__+0x5710c>
   101a0:       3e800593                li      a1,1000
   101a4:       00021537                lui     a0,0x21
   101a8:       19050513                addi    a0,a0,400 # 21190 <__clzdi2+0x48>
   101ac:       26c000ef                jal     ra,10418 <printf>
   101b0:       00000513                li      a0,0
   101b4:       00813083                ld      ra,8(sp)
   101b8:       01010113                addi    sp,sp,16
   101bc:       00008067                ret
```
<p>15 instructions for O1</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_sum1ton.png" alt=debugging O1><br><br>
<h3>Debugging sum1ton.o for Ofast</h3>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
spike pk sum1ton.o
spike -d pk sum1ton.o</code></pre>
<b>Ofast assembly output</b>

```assembly
00000000000100b0 <main>:
   100b0:       0007a637                lui     a2,0x7a
   100b4:       00021537                lui     a0,0x21
   100b8:       ff010113                addi    sp,sp,-16
   100bc:       31460613                addi    a2,a2,788 # 7a314 <__BSS_END__+0x5710c>
   100c0:       3e800593                li      a1,1000
   100c4:       18050513                addi    a0,a0,384 # 21180 <__clzdi2+0x44>
   100c8:       00113423                sd      ra,8(sp)
   100cc:       340000ef                jal     ra,1040c <printf>
   100d0:       00813083                ld      ra,8(sp)
   100d4:       00000513                li      a0,0
   100d8:       01010113                addi    sp,sp,16
   100dc:       00008067                ret
```

<p>12 instructions for Ofast</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_sum1ton.png" alt=debugging Ofast>
</details>	   
<!-- Spike for fact -->	   
<details>
<p><summary>2. Factorial of a Number</summary></p>
<h3>Compiling Factorial C program</h3>
<pre><code>gedit fact.c
gcc fact.c
./a.out</code></pre>

```c
#inlcude<stdio.h>
int main(){
               int fact = 1;
               int i = 1;
               int n = 10;
                   while(i<=n){
                       fact*=i;
                       ++i;
                       }
                printf("Factorial of %d is %d\n",n,fact);
        return 0;
                       }
```
<br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Factorial%20Compilation.png", alt=Factorial Compilation><br><br>
<h3>Debugging fact.o for O1</h3>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></pre>
<b>O1 assembly output</b>

```assembly
0000000000010184 <main>:
   10184:       fe010113                addi    sp,sp,-32
   10188:       00113c23                sd      ra,24(sp)
   1018c:       00813823                sd      s0,16(sp)
   10190:       00913423                sd      s1,8(sp)
   10194:       00100593                li      a1,1
   10198:       00100413                li      s0,1
   1019c:       00b00493                li      s1,11
   101a0:       00040513                mv      a0,s0
   101a4:       03c000ef                jal     ra,101e0 <__muldi3>
   101a8:       0005059b                sext.w  a1,a0
   101ac:       0014041b                addiw   s0,s0,1
   101b0:       fe9418e3                bne     s0,s1,101a0 <main+0x1c>
   101b4:       00058613                mv      a2,a1
   101b8:       00a00593                li      a1,10
   101bc:       00021537                lui     a0,0x21
   101c0:       1b050513                addi    a0,a0,432 # 211b0 <__clzdi2+0x48>
   101c4:       298000ef                jal     ra,1045c <printf>
   101c8:       00000513                li      a0,0
   101cc:       01813083                ld      ra,24(sp)
   101d0:       01013403                ld      s0,16(sp)
   101d4:       00813483                ld      s1,8(sp)
   101d8:       02010113                addi    sp,sp,32
   101dc:       00008067                ret
```

<p>23 instructions for O1</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_O1_factorial.png",alt=Debug O1><br><br>
<h3>Debugging fact.o for Ofast</h3>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o fact.o fact.c
spike pk fact.o
spike -d pk fact.o</code></pre>
<b>Ofast assembly output</b>  

```assembly
00000000000100b0 <main>:
   100b0:       00376637                lui     a2,0x376
   100b4:       00021537                lui     a0,0x21
   100b8:       ff010113                addi    sp,sp,-16
   100bc:       f0060613                addi    a2,a2,-256 # 375f00 <__BSS_END__+0x352cf8>
   100c0:       00a00593                li      a1,10
   100c4:       18050513                addi    a0,a0,384 # 21180 <__clzdi2+0x44>
   100c8:       00113423                sd      ra,8(sp)
   100cc:       340000ef                jal     ra,1040c <printf>
   100d0:       00813083                ld      ra,8(sp)
   100d4:       00000513                li      a0,0
   100d8:       01010113                addi    sp,sp,16
   100dc:       00008067                ret
```

<p>12 instructions for Ofast</p><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%202/Spike_Ofast_factorial.png",alt=Ofast debug><br><br>
</details></details><hr>   
<!--End of Task 2-->
<!-- Task 3 -->   
<details><summary><b>Task 3:</b> Identify instruction type of all the instructions of the Object dump with its exact 32 bits instruction code in the desired instruction type format.</summary><br>
<details><p><summary>RISC-V Instruction Formats</summary></p>
<!-- Explaination -->	
<h2>Instruction Types and Fields</h2>
<p> The RISC-V instructions are categorized into types based on their filed organization.Each type has specific fields like opcode,funct3,funct4,immediate values and register numbers. The types include:</p>
	<ul>
		<li><b>R-Type:</b> Register Type</li>
		<li><b>I-Type:</b> Immediate Type</li>
		<li><b>S-Type:</b> Store Type</li>
		<li><b>B-Type:</b> Branch Type</li>
		<li><b>U-Type:</b> Upper Immediate Type</li>
		<li><b>J-Type:</b> Jump Type</li>
	</ul>
<!-- R-Type -->
<h3>RISCV R-Type Instructions</h3>
<p>R-type instructions are used for operations that involve only registers. These instructions typically perform arithmetic, logical, and shift operations.</p>
<b>Format:</b><br>
<pre>
+----------------------------------------------------------------------------------------------------------------------------------+
  funct7[31:25](7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------------------------------+
</pre>
	<ul>
		<li><b>funct7:</b> Further specifies the operation.<br></li>
		<li><b>rs2:</b> Second source register.<br></li>
		<li><b>rs1:</b> First source register.</li>
		<li><b>funct3:</b> Further specifies the operation.</li>
		<li><b>rd:</b> Destination register.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
<!-- I-Type -->
<h3>RISCV I-Type Instructions</h3>
<p>I-Type instructions cover various operations, including immediate arithmetic, load operations, and certain control flow instructions.</p>
<b>Format:</b><br>
<pre>+----------------------------------------------------------------------------------------------------------+
  imm[31:20](12-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+</pre>
	<ul>
		<li><b>imm:</b> Immediate Value.</li>
		<li><b>rs1:</b> First source register.</li>
		<li><b>funct3:</b> Further specifies the operation.</li>
		<li><b>rd:</b> Destination register.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
<!-- S-Type -->
<h3>RISCV S-Type Instructions</h3>
<p>S-type instructions are essential for accessing and manipulating data in memory.Used to store data from a register to memory.</p>
<b>Format:</b><br>
<pre>+--------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31:25](11:5)(7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:7](4:0)(5-bits) | opcode[6:0](7-bits)
+--------------------------------------------------------------------------------------------------------------------------------------------+</pre>
	<ul>
		<li><b>imm:</b> Immediate Value( split into imm[11:5] and imm[4:0]).</li>
		<li><b>rs2:</b> Second source register.</li>
		<li><b>rs1:</b> First source register.</li>
		<li><b>funct3:</b> Further specifies the operation.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
<!-- B-Type -->   
<h3>RISCV B-Type Instructions</h3>
<p>B-type instructions are crucial for implementing control flow in programs, enabling conditional execution of code blocks.Used for conditional branches, which alter the program flow based on a comparison of register values.</p>
<b>Format:</b><br>
<pre>+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](12)(1-bit) | imm[30:25](10:5)(6-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:8](4:1)(4-bits) | imm[7](11)(1-bit) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</pre>
	<ul>
		<li><b>imm:</b> Immediate Value( split into imm[12], imm[10:5], imm[4:1] and imm[11]).</li>
		<li><b>rs2:</b> Second source register.</li>
		<li><b>rs1:</b> First source register.</li>
		<li><b>funct3:</b> Further specifies the operation.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
<!-- U-Type -->
<h3>RISCV U-Type Instructions</h3>
<p>U-Type instructions are used for operations like loading upper immediate (LUI) and adding upper immediate to PC (AUIPC).</p>
<b>Format:</b><br>
<pre>+----------------------------------------------------------------------------------------------------------+
                  imm[31:12](20-bits)                |    rd[11:7](5-bits)      |     opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+</pre>
	<ul>
		<li><b>imm:</b> Upper 20 bits of the immediate value.</li>
		<li><b>rd:</b> Destination register.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
<!-- J-Type -->    
<h3>RISCV J-Type Instructions</h3>
<p>J-type instructions in RISC-V are primarily used for unconditional jumps to specific target addresses within the program.They play a crucial role in controlling the flow of execution by transferring control to a different part of the code.</p>
<b>Format:</b><br>
<pre>+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](20)(1-bit) | imm[30:21](10:1)(10-bits) | imm[20](11)(1-bit) | imm[19:12](19:12)(8-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+</pre>
	<ul>
		<li><b>imm:</b> Immediate Value( split into imm[20], imm[10:1], imm[11] and imm[19:12]).</li>
		<li><b>rd:</b> Destination register.</li>
		<li><b>opcode:</b> Specifies the operation.</li>
	</ul>
</details>
<!-- Machine Codes -->
<details><p><summary>Machine Codes for Different Instructions</summary></p>
<h2>Machine Codes:</h2>

```assembly
0000000000010184 <main>:
   10184:       fe010113                addi    sp,sp,-32
   10188:       00113c23                sd      ra,24(sp)
   1018c:       00813823                sd      s0,16(sp)
   10190:       00913423                sd      s1,8(sp)
   10194:       00100593                li      a1,1
   10198:       00100413                li      s0,1
   1019c:       00b00493                li      s1,11
   101a0:       00040513                mv      a0,s0
   101a4:       03c000ef                jal     ra,101e0 <__muldi3>
   101a8:       0005059b                sext.w  a1,a0
   101ac:       0014041b                addiw   s0,s0,1
   101b0:       fe9418e3                bne     s0,s1,101a0 <main+0x1c>
   101b4:       00058613                mv      a2,a1
   101b8:       00a00593                li      a1,10
   101bc:       00021537                lui     a0,0x21
   101c0:       1b050513                addi    a0,a0,432 # 211b0 <__clzdi2+0x48>
   101c4:       298000ef                jal     ra,1045c <printf>
   101c8:       00000513                li      a0,0
   101cc:       01813083                ld      ra,24(sp)
   101d0:       01013403                ld      s0,16(sp)
   101d4:       00813483                ld      s1,8(sp)
   101d8:       02010113                addi    sp,sp,32
   101dc:       00008067                ret
```
<!-- 1 -->
<h3>1. Machine code for <code>addi sp, sp, -32</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addi sp, sp, -32</code><br><br>
	   <ul>
		   <li><b>Opcode: </b>0010011 (7 bits) </li>
		   <li><b>Immediate: </b>-32 (12 bits,two's complement) </li>
		   <li><b>Source Register(rs1): </b>sp (x2,5 bits) </li>
		   <li><b>Destination Register(rd): </b>sp (x2,5 bits)</li>
		   <li><b>Function(funct3): </b>000 (3 bits)</li>
	   </ul>	   
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(-32): </b><code>111111100000</code></li>
		   <li><b>rs1(sp=x2): </b><code>00010</code> </li>
		   <li><b>funct3: </b><code>000</code></li>
		   <li><b>rd(sp=x2): </b><code>00010</code> </li>
		   <li><b>Opcode: </b><code>0010011</code></li>
	   </ul>   
    
```assembly 
10184:       fe010113          addi  sp, sp, -32
```   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>111111100000</td>
		<td>00010</td>
		<td>000</td>
		<td>00010</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 2 -->
<h3>2. Machine code for <code>sd ra, 24(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd ra, 24(sp)</code><br><br>  
	   <ul>
		   <li><b>Opcode: </b>0100011 (7 bits)</li>
		   <li><b>Immediate: </b>24 (12 bits split into imm[11:5] and imm[4:0]) </li>
		   <li><b>Base Register(rs1): </b>sp (x2,5 bits)</li>
		   <li><b>Source Register(rd): </b>ra (x1,5 bits)</li>
		   <li><b>Function(funct3): </b>011 (3 bits)</li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(24): </b><code>000000011000 </code>(Split into imm[11:5]=<code>0000000</code> and imm[4:0]=<code>11000</code>)</li>
		   <li><b>rs1(sp=x2): </b><code>00010</code></li>
		   <li><b>funct3: </b><code>011</code> </li>
		   <li><b>rs2(ra=x1): </b><code>00001</code> </li>
		   <li><b>Opcode: </b><code>0100011</code></li>
	   </ul>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
	   <ul>
		   <li><b>imm[11:5] (7 bits): </b><code>0000000</code></li>
		   <li><b>rs2 (5 bits): </b><code>00001</code></li>
		   <li><b>rs1 (5 bits): </b><code>00010</code></li>
		   <li><b>funct3 (3 bits): </b><code>011</code></li>
		   <li><b>imm[4:0] (5 bits): </b><code>11000</code></li>
		   <li><b>opcode (7 bits): </b><code>0100011</code></li>
	   </ul>
    
```assembly
10188:       00113c23       sd   ra, 24(sp)
```
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>00001</td>
		<td>00010</td>
		<td>011</td>
		<td>11000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 3 -->
<h3>3. Machine code for <code>sd s0, 16(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s0, 16(sp)</code><br><br>
	   <ul>
		   <li><b>Opcode: </b>0100011 (7 bits) </li>
		   <li><b>Immediate: </b>16 (12 bits split into imm[11:5] and imm[4:0])</li>
		   <li><b>Base Register(rs1): </b>sp (x2,5 bits)</li>
		   <li><b>Source Register(rd): </b>s0 (x8,5 bits)</li>
		   <li><b>Function(funct3): </b>011 (3 bits)</li>
	   </ul> 
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(16): </b><code>000000010000 </code>(Split into imm[11:5]=<code>0000000</code> and imm[4:0]=<code>10000</code>)</li>
		   <li><b>rs1(sp=x2): </b><code>00010</code> </li>
		   <li><b>funct3: </b><code>011</code></li>
		   <li><b>rs2(s0=x8): </b><code>01000</code> </li>
		   <li><b>Opcode: </b><code>0100011</code></li>
	   </ul>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
	   <ul>
		   <li><b>imm[11:5] (7 bits): </b><code>0000000</code></li>
		   <li><b>rs2 (5 bits): </b><code>01000</code></li>
		   <li><b>rs1 (5 bits): </b><code>00010</code></li>
		   <li><b>funct3 (3 bits): </b><code>011</code></li>
		   <li><b>imm[4:0] (5 bits): </b><code>10000</code></li>
		   <li><b>opcode (7 bits): </b><code>0100011</code></li>
	   </ul>
    
```assembly
1018c:       00813823           sd     s0, 16(sp)
```
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>01000</td>
		<td>00010</td>
		<td>011</td>
		<td>10000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 4 -->
<h3>4. Machine code for <code>sd s1, 8(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s1, 8(sp)</code><br><br>  
	   <ul>
		   <li><b>Opcode: </b>0100011 (7 bits) </li>
		   <li><b>Immediate: </b>8 (12 bits split into imm[11:5] and imm[4:0])</li>
		   <li><b>Base Register(rs1): </b>sp (x2,5 bits) </li>
		   <li><b>Source Register(rd): </b>s1 (x9,5 bits) </li>
		   <li><b>Function(funct3): </b>011 (3 bits)</li>
	   </ul> 
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(8): </b><code>000000001000 </code>(Split into imm[11:5]=<code>0000000</code> and 		imm[4:0]=<code>01000</code>)</li>
		   <li><b>rs1(sp=x2): </b><code>00010</code></li>
		   <li><b>funct3: </b><code>011</code></li>
		   <li><b>rs2(s1=x9): </b><code>01001</code></li>
		   <li><b>Opcode: </b><code>0100011</code> </li>
	   </ul>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
	   <ul>
		   <li><b>imm[11:5] (7 bits): </b><code>0000000</code></li>
		   <li><b>rs2 (5 bits): </b><code>01001</code></li>
		   <li><b>rs1 (5 bits): </b><code>00010</code></li>
		   <li><b>funct3 (3 bits): </b><code>011</code></li>
		   <li><b>imm[4:0] (5 bits): </b><code>01000</code></li>
		   <li><b>opcode (7 bits): </b><code>0100011</code></li>
	   </ul>

```assembly
10190:       00913423           sd    s1, 8(sp)
```
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>01001</td>
		<td>00010</td>
		<td>011</td>
		<td>01000</td>
		<td>0100011</td>
	</tr>
</table>
<!-- 5 -->
<h3>5. Machine code for <code>li a1, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li a1, 1</code> <br><br> 
	   <ul>
		   <li><b>Opcode: </b>0010011 (7 bits) </li>
		   <li><b>Immediate: </b>1 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>zero (x0,5 bits) </li>
		   <li><b>Destination Register(rd): </b>a1 (x11,5 bits)</li>
		   <li><b>Function(funct3): </b>000 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000000001</code> </li>
		   <li><b>rs1(zero=x0): </b><code>00000</code></li>
		   <li><b>funct3: </b><code>000</code> </li>
		   <li><b>rd(a1=x11): </b><code>01011</code></li>
		   <li><b>Opcode: </b><code>0010011</code> </li>
	   </ul>
    
```assembly
10194:       00100593          li    a1, 1
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>00000</td>
		<td>000</td>
		<td>01011</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 6 -->
<h3>6. Machine code for <code>li s0, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s0, 1</code> <br><br>
	<ul>
		<li><b>Opcode: </b>0010011 (7 bits) </li>
		<li><b>Immediate: </b>1 (12 bits) </li>
		<li><b>Source Register(rs1): </b>zero (x0,5 bits) </li>
		<li><b>Destination Register(rd): </b>s0 (x8,5 bits)</li>
		<li><b>Function(funct3): </b>000 (3 bits) </li>
	</ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000000001</code></li>
		   <li><b>rs1(zero=x0): </b><code>00000</code></li>
		   <li><b>funct3: </b><code>000</code></li>
		   <li><b>rd(s0=x8): </b><code>01000</code></li>
		   <li><b>Opcode: </b><code>0010011</code></li>
	   </ul>
    
```assembly
10198:       00100413            li    s0,1
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>00000</td>
		<td>000</td>
		<td>01000</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 7 -->
<h3>7. Machine code for <code>li s1, 11</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s1, 11</code> <br><br> 
	   <ul>
		   <li><b>Opcode: </b>0010011 (7 bits)</li>
		   <li><b>Immediate: </b>11 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>zero (x0,5 bits) </li>
		   <li><b>Destination Register(rd): </b>s1 (x9,5 bits)</li>
		   <li><b>Function(funct3): </b>000 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000001011</code> </li>
		   <li><b>rs1(zero=x0): </b><code>00000</code></li>
		   <li><b>funct3: </b><code>000</code> </li>
		   <li><b>rd(s1=x9): </b><code>01001</code> </li>
		   <li><b>Opcode: </b><code>0010011</code> </li>
	   </ul>

```assembly
1019c:       00b00493            li     s1, 11
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000001011</td>
		<td>00000</td>
		<td>000</td>
		<td>01001</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 8 -->
<h3>8. Machine code for <code>mv a0, s0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>mv a0, s0</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0010011 (7 bits)</li>
		   <li><b>Immediate: </b>0 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>s0 (x8,5 bits)</li>
		   <li><b>Destination Register(rd): </b>a0 (x10,5 bits) </li>
		   <li><b>Function(funct3): </b>000 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(0): </b><code>000000000000</code></li>
		   <li><b>rs1(s0=x8): </b><code>01000</code> </li>
		   <li><b>funct3: </b><code>000</code></li>
		   <li><b>rd(a0=x10): </b><code>01010</code></li>
		   <li><b>Opcode: </b><code>0010011</code></li>
	   </ul>
    
```assembly
101a0:       00040513            mv    a0, s0
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01000</td>
		<td>000</td>
		<td>01010</td>
		<td>0010011</td>
	</tr>
</table>
<!-- 9 -->
<h3>9. Machine code for <code>sext.w a1, a0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sext.w a1, a0</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0011011 (7 bits) </li>
		   <li><b>Immediate: </b>0 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>a0 (x10,5 bits) </li>
		   <li><b>Destination Register(rd): </b>a1 (x11,5 bits) </li>
		   <li><b>Function(funct3): </b>000 (3 bits)</li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000000000</code></li>
		   <li><b>rs1(a0=x10): </b><code>01010</code> </li>
		   <li><b>funct3: </b><code>000</code> </li>
		   <li><b>rd(a1=x11): </b><code>01011</code></li>
		   <li><b>Opcode: </b><code>0011011</code></li>
	   </ul> 
    
```assembly
101a8:       0005059b          sext.w  a1, a0 
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01010</td>
		<td>000</td>
		<td>01011</td>
		<td>0011011</td>
	</tr>
</table>
<!-- 10 -->
<h3>10. Machine code for <code>addiw s0, s0, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addiw s0, s0, 1</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0011011 (7 bits)</li>
		   <li><b>Immediate: </b>1 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>s0 (x8,5 bits)</li>
		   <li><b>Destination Register(rd): </b>s0 (x8,5 bits)</li>
		   <li><b>Function(funct3): </b>000 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000000001</code></li>
		   <li><b>rs1(s0=x8): </b><code>01000</code></li>
		   <li><b>funct3: </b><code>000</code></li>
		   <li><b>rd(s0=x8): </b><code>01000</code></li>
		   <li><b>Opcode: </b><code>0011011</code></li>
	   </ul>

```assembly
101ac:       0014041b          addiw   s0, s0, 1
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>01000</td>
		<td>000</td>
		<td>01000</td>
		<td>0011011</td>
	</tr>
</table>
<!-- 11 -->
<h3>11. Machine code for <code>lui a0, 0x21</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>lui a0, 0x21</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0110111 (7 bits)</li>
		   <li><b>Immediate: </b>0x21(33) (20 bits) </li>
		   <li><b>Destination Register(rd): </b>a0 (x10,5 bits)</li>
	   </ul> 
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(0x21): </b><code>00000000000000100001</code></li>
		   <li><b>rd(a0=x10): </b><code>01010</code> </li>
		   <li><b>Opcode: </b><code>0110111</code> </li>
	   </ul>

```assembly
101bc:       00021537          lui  a0, 0x21
```
<table>
	<tr>
		<th>Immediate (20 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>00000000000000100001</td>
		<td>01010</td>
		<td>0110111</td>
	</tr>
</table>
<!-- 12 -->
<h3>12. Machine code for <code>ld ra, 24(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld ra, 24(sp)</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0000011 (7 bits) </li>
		   <li><b>Immediate: </b>24 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>sp (x2,5 bits)</li>
		   <li><b>Destination Register(rd): </b>ra (x1,5 bits)</li>
		   <li><b>Function(funct3): </b>011 (3 bits)</li>
	   </ul> 
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(24): </b><code>000000011000</code> </li>
		   <li><b>rs1(sp=x2): </b><code>00010</code> </li>
		   <li><b>funct3: </b><code>011</code> </li>
		   <li><b>rd(ra=x1): </b><code>00001</code></li>
		   <li><b>Opcode: </b><code>0000011</code></li>
	   </ul> 	 

```assembly
101cc:       01813083          ld   ra, 24(sp)
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000011000</td>
		<td>00010</td>
		<td>011</td>
		<td>00001</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 13 -->
<h3>13. Machine code for <code>ld s0, 16(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld s0, 16(sp)</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0000011 (7 bits) </li>
		   <li><b>Immediate: </b>16 (12 bits)</li>
		   <li><b>Source Register(rs1): </b>sp (x2,5 bits) </li>
		   <li><b>Destination Register(rd): </b>s0 (x8,5 bits)</li>
		   <li><b>Function(funct3): </b>011 (3 bits)</li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(16): </b><code>000000010000</code> </li>
		   <li><b>rs1(sp=x2): </b><code>00010</code></li>
		   <li><b>funct3: </b><code>011</code></li>
		   <li><b>rd(s0=x8): </b><code>01000</code> </li>
		   <li><b>Opcode: </b><code>0000011</code></li>
	   </ul> 

```assembly
101d0:       01013403          ld   s0, 16(sp)
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000010000</td>
		<td>00010</td>
		<td>011</td>
		<td>01000</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 14 -->
<h3>14. Machine code for <code>ld s1, 8(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld s1, 8(sp)</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>0000011 (7 bits) </li>
		   <li><b>Immediate: </b>8 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>sp (x2,5 bits) </li>
		   <li><b>Destination Register(rd): </b>s1 (x9,5 bits) </li>
		   <li><b>Function(funct3): </b>011 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(8): </b><code>000000001000</code></li>
		   <li><b>rs1(sp=x2): </b><code>00010</code></li>
		   <li><b>funct3: </b><code>011</code></li>
		   <li><b>rd(s1=x9): </b><code>01001</code> </li>
		   <li><b>Opcode: </b><code>0000011</code></li>
	   </ul> 

```assembly
101d4:       00813483          ld   s1, 8(sp)
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000001000</td>
		<td>00010</td>
		<td>011</td>
		<td>01001</td>
		<td>0000011</td>
	</tr>
</table>
<!-- 15 -->
<h3>15. Machine code for <code>ret</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ret</code>  <br><br>
	   <ul>
		   <li><b>Opcode: </b>1100111 (7 bits) </li>
		   <li><b>Immediate: </b>0 (12 bits) </li>
		   <li><b>Source Register(rs1): </b>ra (x1,5 bits)</li>
		   <li><b>Destination Register(rd): </b>zero (x0,5 bits) </li>
		   <li><b>Function(funct3): </b>000 (3 bits) </li>
	   </ul>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	   <ul>
		   <li><b>Immediate(1): </b><code>000000001011</code></li>
		   <li><b>rs1(ra=x1): </b><code>00001</code></li>
		   <li><b>funct3: </b><code>000</code> </li>
		   <li><b>rd(zero=x0): </b><code>00000</code></li>
		   <li><b>Opcode: </b><code>1100111</code></li>
	   </ul>

```assembly
101dc:       00008067       ret
```
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>00001</td>
		<td>000</td>
		<td>00000</td>
		<td>1100111</td>
	</tr>
</table>
</details>
</details>
<hr>
<!--End of Task 3-->
<!-- Task 4 -->
<details><summary><b>Task 4: </b>By using RISC-V Core: Verilog netlist and Testbench, perform an experiment of Functional Simulation using GTKWave and Observe the waveforms.</summary>
<h3>Steps:</h3>
1. Using suitable commands install the iverilog and GTKWave in ubuntu<br>
2. Compile the RISC-V Core: Verilog netlist and Testbench<br>
3. Observe the waveform output in GTKWave window<br>
<h4>Installing iverilog and GTKWave in Ubuntu:</h4>
	
```bash
sudo apt install iverilog gtkwave
```
<h3>Simulate and run the verilog code</h3>

```bash
iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
./iiitb_rv32i
gtkwave iiitb_rv32i.vcd
```
<h4>GTKWave Window:</h4><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/GTKWave_Window.png" alt="GTKWave Window">
<br><br>
<h4>Hardcoded Instructions:</h4><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/Instructions.png" alt="Hardcoded ISA">
<br>
<h3>Ouput Waveforms:</h3>
<p>The output waveforms showing the instructions performed in a 5-stage pipelined architecture</p>
<b><i>Instruction 1:</i></b><pre> ADD R6, R2, R1</pre>
	<p>This instruction Adds values of registers R2 and R1 and stores the result in register R6, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/01_add_r6_r1_r2.png" alt="ADD R6, R2, R1">
<br><br><b><i>Instruction 2:</i></b><pre> SUB R7, R1, R2</pre>
	<p>This instruction subtracts value of register R2 from R1 and stores the result in register R7, In this case 1 - 2 = -1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/02_sub_r7_r1_r2.png" alt="SUB R7, R1, R2">
<br><br><b><i>Instruction 3:</i></b><pre> AND R8, R1, R3</pre>
	<p>This instruction executes bitwise "AND" between values of registers R1 and R3 and stores the result in register R8, In this case 01 & 11 = 01(1 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/03_and_r8%2Cr1%2Cr3.png" alt="AND R8, R1, R3">
<br><br><b><i>Instruction 4:</i></b><pre> OR R9, R2, R5</pre>
	<p>This instruction executes bitwise "OR" between values of registers R2 and R5 and stores the result in register R9, In this case 010 | 101 = 111(7 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/04_or_r9_r2_r5.png" alt="OR R9, R2, R5">
<br><br><b><i>Instruction 5:</i></b><pre> XOR R10, R1, R4</pre>
	<p>This instruction executes bitwise XOR between values of registers R1 and R4 and stores the result in register R10, In this case 001 ^ 100 = 101(5 in decimal).</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/05_xor_r10_r1_r4.png" alt="XOR R10, R1, R4">
<br><br><b><i>Instruction 6:</i></b><pre> SLT R11, R2, R4</pre>
	<p>This instruction checks the values of registers R2 and R4 if value of R2 is less than value of R4, then register R11 is set to 1, In this case 2<4 so R11 is set to 1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/06_slt_r11_r2_r4.png" alt="SLT R11, R2, R4">
<br><br><b><i>Instruction 7:</i></b><pre> ADDI R12, R4, 5</pre>
	<p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/07_addi_r12_r4_5.png" alt="ADDI R12, R4, 5">
<br><br><b><i>Instruction 8:</i></b><pre> SW R3, R1, 2</pre>
	<p>This instruction stores the register data @R1+2 into the memory, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/08_sw_r3_r1_2.png" alt="SW R3, R1, 2">
<br><br><b><i>Instruction 9:</i></b><pre> LW R13, R1, 2</pre>
	<p>This instruction loads the register data @R1+2 into the register R13, In this case 1 + 2 = 3.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/09_lw_r13_r1_2.png" alt="LW R13, R1, 2">
<br><br><b><i>Instruction 10:</i></b><pre> BEQ R0, R0, 15</pre>
	<p>This instruction Branches to 15 instructions ahead of current instruction if values of registers R0 equals R0, so Program Counter will be incremented by 15, In this case PC is 10 so new PC value will be 10+15=25.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/10_beq_r0_r0_15.png" alt="BEQ R0, R0, 15">
<br><br><b><i>Instruction 11:</i></b><pre> ADD R14, R2 R2</pre>
	<p> This instruction Adds values of registers R2 and R2 and stores the result in register R14, In this case 2 + 2 = 4.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/11_add_r14_r2_r2.png" alt="ADD R14, R2 R2">
<br><br><b><i>Instruction 12:</i></b><pre> BNE R0, R1, 20</pre>
	<p>This instruction Branches to 20 instructions ahead of current instruction if values of registers R0 and R1 don't match , so Program Counter will be incremented by 20, In this case PC is 28 so new PC value will be 28+20=48.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/12_bne_r0_r1_20.png" alt="BNE R0, R1, 20">
<br><br><b><i>Instruction 13:</i></b><pre> ADDI R12, R4, 5</pre>
	<p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/13_addi_r12_r4_5.png" alt="ADDI R12, R4, 5">
<br><br><b><i>Instruction 14:</i></b><pre> SLL R15, R1, R2</pre>
	<p>This instruction shifts the value of register R1 to left by 2, (001)&lt;&lt;2=(100)4.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/14_sll_r15_r1_r2.png" alt="SLL R15, R1, R2">
<br><br><b><i>Instruction 15:</i></b><pre> SRL R16, R4, R2</pre>
	<p>This instruction shifts the value of register R1 to right by 2, (100)&gt;&gt;2=(001)1.</p>
	<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%204/15_srl_r16_r4_r2.png" alt="SRL R16, R4, R2">
<br><br>
</details>
<!--End of Task 4-->
<hr>
<!-- Task 5 -->
<details>
	<summary><b>Task 5:</b> To implement any digital circuit using VSDSquadron Mini and check whether building and uploading of C program file on RISCV processor works.</summary>
<h2>Implement 4 by 4 Multiplier Using VSDSquadron Mini </h2>
<h3>Overview</h3>
	<p>This project involves the implementation of a 4x4 binary multiplier circuit using the VSD Squadron Mini, a RISC-V based SoC development kit. A binary multiplier is a fundamental digital circuit that performs binary multiplication of two numbers. This project showcases the practical application of digital logic and RISC-V architecture by implementing a multiplication function. It involves reading and writing binary data through GPIO pins, implementing the 4x4 multiplier logic , simulating the design using the PlatformIO IDE, and displaying the multiplier's 8-bit output using LEDs. This project provides a hands-on understanding of how to control and manipulate digital signals using a microcontroller and how to implement a more complex digital building block like a multiplier.  It also highlights the use of RISC-V for custom hardware acceleration or digital signal processing applications.</p>
<h3>Components Required</h3>
	<ul>
		<li> VSD Squadron Mini</li>
		<li> Push buttons for A input B input and Reset </li>
		<li> 8 LEDs for Output </li>
		<li> Bread Board</li>
		<li> Jumper wires</li>
		<li> VS Code for software Development</li>
		<li> PlatformIO multi framework professional IDE</li>
	</ul>
<h3>Hardware Connections</h3>
	<ul>
		<li><b>Inputs: </b>Three inputs connected to the GPIO Pins of VSDsquadron Mini via push buttons mounted on the breadboard.</li>
		<li><b>Outputs: </b> Eight LEDs are connected to display the result of 4 by 4 Binary Multiplier.</li>
		<li>The GPIO pins are configured according to the reference mannual ensuring the correct flow of signals between the components.</li>
	</ul><br>
<img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%205/4_by_4_Multiplier_Circuit.png" alt="4 by 4 Multiplier">
<br><br>
<h3>Working and Block Diagram</h3>
<ol type="1">
	<li><b>Physical Circuit:</b> Push button for A increments the value of A and push button for B increments the value of B and it is coded to multiply A and B to get the output in 8 LEDs as the output will be 8 bits in size.</li>
	<li><b>Partial Product Generation (using AND gates):</b>
		<ul>
			<li>Each bit of the multiplier (B) is ANDed with each bit of the multiplicand (A).</li>
			<li>For example, <code>A0 AND B0</code> produces the least significant bit of the first partial product. <code>A3 AND B2</code> produces the most significant bit of another partial product, and so on.</li>
			<li>Since we have 4 bits in A and 4 bits in B, we get a total of 4 x 4 = 16 partial products. However, notice how they are organized for addition.</li>
		</ul>
	</li>
	<li><b>Partial Product Organization and Shifting:</b>
		<ul>
			<li>partial products are arranged such that the appropriate bits are aligned for addition. This implicitly handles the "shifting" that is necessary in multiplication.</li>
			<li>outputs of the AND gates leading into the adders.they shift left as we go down the circuit. This is equivalent to multiplying by a power of 2 (just like in decimal multiplication when you shift left).</li>
		</ul>
	</li>
	<li><b>Adding the Partial Products (using 4-bit Adders):</b>
		<ul>
			<li>4-bit adders are used to sum the partial products in stages.</li>
			<li><b>First Stage:</b> The first row of AND gates' outputs are directly passed as inputs to the first 4-bit adder. The other input to this adder is zero.</li>
			<li><b>Subsequent Stages:</b>The outputs (sum and carry) of each 4-bit adder are then fed into the next 4-bit adder along with the next set of partial products. This process continues until all partial products are summed.</li>
		</ul>
	</li>
	<li><b>Final Product:</b>
		<ul>
			<li>The final 8-bit product (P7 to P0) is obtained as the output of the last 4-bit adder stage.</li>
		</ul>
	</li>
</ol>
	<br><img src="https://github.com/akshaykumartallur/samsung-riscv/blob/main/Task%205/4_bit_Multiplier_Block_Diagram.png" alt="Block_Diagram_Multiplier"><br>
<h3>Truth Table for 4 By 4 Multiplier</h3>
<table>
<!--Row 1-->
	<tr>
		<th colspan="4" align="center">A</th><th colspan="4" align="center">B</th><th colspan="8" align="center">P</th>
	</tr>
<!--Row 2-->
<tr> 
<!--A -->  <th>A<sub>3</sub></th> <th>A<sub>2</sub></th> <th>A<sub>1</sub></th> <th>A<sub>0</sub></th> 
<!--B -->  <th>B<sub>3</sub></th> <th>B<sub>2</sub></th> <th>B<sub>1</sub></th> <th>B<sub>0</sub></th>
<!--Product-->	<th>P<sub>7</sub></th> <th>P<sub>6</sub></th> <th>P<sub>5</sub></th> <th>P<sub>4</sub></th> 
		<th>P<sub>3</sub></th> <th>P<sub>2</sub></th> <th>P<sub>1</sub></th> <th>P<sub>0</sub></th>
</tr>	
<!--Row 3-->
<tr> 
<!--A -->  <td>0</td> <td>0</td> <td>0</td> <td>0</td> 
<!--B -->  <td>0</td> <td>0</td> <td>0</td> <td>0</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>	
<!--Row 4-->
<tr> 
<!--A -->  <td>0</td> <td>0</td> <td>0</td> <td>1</td> 
<!--B -->  <td>0</td> <td>0</td> <td>0</td> <td>1</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 5-->
<tr> 
<!--A -->  <td>0</td> <td>0</td> <td>1</td> <td>0</td> 
<!--B -->  <td>0</td> <td>0</td> <td>1</td> <td>0</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td>
</tr>
<!--Row 6-->
<tr> 
<!--A -->  <td>0</td> <td>0</td> <td>1</td> <td>1</td> 
<!--B -->  <td>0</td> <td>0</td> <td>1</td> <td>1</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 7-->
<tr> 
<!--A -->  <td>0</td> <td>1</td> <td>0</td> <td>0</td> 
<!--B -->  <td>0</td> <td>1</td> <td>0</td> <td>0</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>
<!--Row 8-->
<tr> 
<!--A -->  <td>0</td> <td>1</td> <td>0</td> <td>1</td> 
<!--B -->  <td>0</td> <td>1</td> <td>0</td> <td>1</td>
<!--Product-->	<td>0</td> <td>0</td> <td>0</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 9-->
<tr> 
<!--A -->  <td>0</td> <td>1</td> <td>1</td> <td>0</td> 
<!--B -->  <td>0</td> <td>1</td> <td>1</td> <td>0</td>
<!--Product-->	<td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td>
</tr>
<!--Row 10-->
<tr> 
<!--A -->  <td>0</td> <td>1</td> <td>1</td> <td>1</td> 
<!--B -->  <td>0</td> <td>1</td> <td>1</td> <td>1</td>
<!--Product-->	<td>0</td> <td>0</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 11-->
<tr> 
<!--A -->  <td>1</td> <td>0</td> <td>0</td> <td>0</td> 
<!--B -->  <td>1</td> <td>0</td> <td>0</td> <td>0</td>
<!--Product-->	<td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>
<!--Row 12-->
<tr> 
<!--A -->  <td>1</td> <td>0</td> <td>0</td> <td>1</td> 
<!--B -->  <td>1</td> <td>0</td> <td>0</td> <td>1</td>
<!--Product-->	<td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 13-->
<tr> 
<!--A -->  <td>1</td> <td>0</td> <td>1</td> <td>0</td> 
<!--B -->  <td>1</td> <td>0</td> <td>1</td> <td>0</td>
<!--Product-->	<td>0</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td>
</tr>
<!--Row 14-->
<tr> 
<!--A -->  <td>1</td> <td>0</td> <td>1</td> <td>1</td> 
<!--B -->  <td>1</td> <td>0</td> <td>1</td> <td>1</td>
<!--Product-->	<td>0</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 15-->
<tr> 
<!--A -->  <td>1</td> <td>1</td> <td>0</td> <td>0</td> 
<!--B -->  <td>1</td> <td>1</td> <td>0</td> <td>0</td>
<!--Product-->	<td>1</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>
<!--Row 16-->
<tr> 
<!--A -->  <td>1</td> <td>1</td> <td>0</td> <td>1</td> 
<!--B -->  <td>1</td> <td>1</td> <td>0</td> <td>1</td>
<!--Product-->	<td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
<!--Row 17-->
<tr> 
<!--A -->  <td>1</td> <td>1</td> <td>1</td> <td>0</td> 
<!--B -->  <td>1</td> <td>1</td> <td>1</td> <td>0</td>
<!--Product-->	<td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td> <td>0</td> <td>0</td>
</tr>
<!--Row 18-->
<tr> 
<!--A -->  <td>1</td> <td>1</td> <td>1</td> <td>1</td> 
<!--B -->  <td>1</td> <td>1</td> <td>1</td> <td>1</td>
<!--Product-->	<td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>1</td>
</tr>
</table>
	
<h3>Program</h3>

```c
//4 by 4 Multiplier
#include<stdio.h>
#include<debug.h>
#include<ch32v00x.h>
	
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; // structure variable used for GPIO configuration
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to enable the clock for port D
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE); // to enable the clock for port C
	
// 3 inputs A,B and Reset
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0| GPIO_Pin_1| GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; 
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    
// 4 outputs from C port for bit0,bit1,bit2,bit3
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3| GPIO_Pin_4 |GPIO_Pin_5| GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; 
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    
//4 outputs from D port for bit4,bit5,bit6,bit7
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_4 | GPIO_Pin_5;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; 
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
}
int main()
{
    uint8_t a=0;
    uint8_t b=0;
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();
while(1)
    {
        uint8_t curStateA=SET;
        uint8_t prevStateA=SET;
        uint8_t curStateB=SET;
        uint8_t prevStateB=SET;
        curStateA = GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_0);
        curStateB = GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_1);
	
//reset logic
            if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_2)==RESET){
                Delay_Ms(30);
                while(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_2)==RESET);
                a=0;
                b=0;
            }
	    
//This is to increment the value of a on each push
            if(curStateA != prevStateA && curStateA==RESET){
                Delay_Ms(30);
                curStateA=GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_0);
                if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_0)==RESET){
                    a++;
                    while(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_0)==RESET);
                }
            }
	    
//This is to increment the value of b on each push
            if(curStateB != prevStateB && curStateB==RESET){
                Delay_Ms(30);
                curStateB=GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_1);
                if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_1)==RESET){
                    b++;
                    while(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_1)==RESET);
                }
            }
            uint8_t mul=a*b;
            GPIO_WriteBit(GPIOC, GPIO_Pin_3, (mul & 1)?SET:RESET);
            GPIO_WriteBit(GPIOC, GPIO_Pin_4, (mul & 2)?SET:RESET);
            GPIO_WriteBit(GPIOC, GPIO_Pin_5, (mul & 4)?SET:RESET);
            GPIO_WriteBit(GPIOC, GPIO_Pin_6, (mul & 8)?SET:RESET);
            GPIO_WriteBit(GPIOD, GPIO_Pin_5, (mul & 16)?SET:RESET);
            GPIO_WriteBit(GPIOD, GPIO_Pin_2, (mul & 32)?SET:RESET);
            GPIO_WriteBit(GPIOD, GPIO_Pin_3, (mul & 64)?SET:RESET);
            GPIO_WriteBit(GPIOD, GPIO_Pin_4, (mul & 128)?SET:RESET);
            Delay_Ms(100);
    }
}
```

</details> 
<hr>
<h3>4 by 4 Multiplier Implementation Video</h3>
<p><b>Higher Quality Video</b></p>
<ul>
	<li><a href="https://drive.google.com/file/d/1_pS-wEMCzns7ERaU2UgeH8P4fgFpzNvg/view?usp=sharing"><b>Drive Link<i></i></b></a></li>
	<li><a href="https://youtu.be/0Qlu3d7ys3I?si=OJO6Nf1cwpFqs78j"><b>YouTube Link<i></i></b></a></li>
</ul>

https://github.com/user-attachments/assets/a26b2842-a40c-455f-afe3-1f0b9faee3d0

