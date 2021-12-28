### Introduction

### PLC Counters :

<p style="text-align: justify;">A counter is a simple device intended to do one simple thing-count. Every PLC has counter instructions. Using counters sometimes be little challenging because many manufacturers seem to use them different way. In other words, the instruction symbol used and method of programming will change for different manufacturers. <br>

A typical counter counts from 0 up to a predetermined value, called the “preset” value. For example, if you wanted to count from certain value, say from 0 to 50, you would be counting up using a count-up or up-counter. Here, the number “50”, is the predetermined value, which is nothing but preset value. The current count or accumulated count is called as the “accumulated value”. If our counter had counted 25 pieces that had passed on the conveyor, the accumulated count would be 25. When all 50 pieces had passed on the conveyor, the preset value and counter accumulated value would be equal. At this point the counter would signal other logic within the PLC program that the batch of 50 was completed and it should now take some action. The next action the PLC has to take is to move the box containing 50 parts on to the next station for carton sealing. To start counting the next batch, a reset instruction would be used to reset the counter’s accumulated value back to zero.<br>
The fig below shows an up-counter, counting from 0 to 50. </p>

<center>
<img src="images/counter/1.gif" style="width:485px; height:307px;"></center>
<br>
The fig below shows a down-counter, counting from 50 to 0.

<center>
<img src="images/counter/2.gif" style="width:503px; height:289px;"></center>
<br>

<center>
<img src="images/counter/3.gif" style="width:463px; height:452px;"></center>

#### As for explanation, let us take ALLEN-BRADELY counters :
<p style="text-align: justify;">In Allen-bradely counter, the default counter file is file 5. The counter data is stored in counter file. Each counter consists of three 16-bit words and is known as “counter element”. In a single processor file, there can be many counter files. Any data file, which is greater than file 8 can be assigned as an additional counter file. Each counter file can have up to 256 counter elements.<br>

A counter instruction is one element. A counter element is made up of three 16-bit words. Thus, the counter instruction contains the three parts i.e. word0, word1 and word2. </p>

<ul type=disc style="text-align: justify;">
<li>Word zero is for status bits. Status bits include CU, CD, DN, OV, UN, and UA. Along with their associated instructions.</li>
<li>Word one is for the preset value.</li>
<li>Word two is for accumulated value.</li>
</ul>

<center>
<img src="images/counter/4.gif" style="width:501px; height:129px;"></center>

#### Addressing a counter :
<ol type="1">
<li><p style="text-align: justify;">To address the counter as a unit the addressing format used is C5:4.<br>
Where, C= C identifies this as a counter file.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 5= This is counter file 4 which is default. Any unused file from 10 to 255can be assigned to counters.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :4= The colon used here is called the file separator. It separates the file, file 5, from the specific counter, in this case, counter 4 in counter file 5.
</p>
</li>

<li>
<p style="text-align: justify;">To address the counter 14’s accumulated value, the address used is C5:14.ACC.<br>
Where, C= C identifies this as a counter file.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 5= This represents the counter file 5.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; :14= The colon is called the file separator. The colon separates the file, file 5, from the specific counter, in this case , counter 14 in counter file 5.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; . = The point is called the word delimiter. The word delimiter is used to separate the counter number, called the structure, from the sub element. The sub element is ACC for the accumulated value. Similarly, preset value can be accessed as C5:14.PRE.
</p>
</ol>

##### Let us see the word zero of the counter element:
<ul type=disc style="text-align: justify;">
<li>In word zero, bit 10, is the update accumulator bit, which is identified as UA. One speciality about UA bit is that , it is only available on a fixed- style SLC 500 PLC. This bit is used in conjunction with the high speed counter built into the fixed SLCs.</li>
<li>Bit 11 represents UN bit, which is known as underflow bit. Underflow bit is set when the accumulated value of a count-down counter has reached the lowest possible accumulated value, that is -32,768. The counter will automatically wrap around. Then it starts counting down from maximum positive value, +32,767.</li>
<li>Bit 12 represents the overflow bit, OV. This bit is set when the accumulated value of a count-up counter has reached the highest possible accumulated value, which is +32,767. At this point, the counter will automatically wrap around and starts counting up from the maximum negative value, -32,768.</li>
<li>Bit 13 is DN, which is known as done bit. This bit is set when the counter’s accumulated value is equal to or greater than the preset value.</li>
<li>Bit 14 is CD, which is known as count-down-enabled bit. CD bit is set when counting down and the rung conditions are true.</li>
<li>Bit 15 is CU, which is known as count-up-enabled bit. CU bit is set when counting up and the rung conditions are true.</li>
</ul>

##### The addressing format of counter status bits is as follows:
<ul type=disc style="text-align: justify;">
<li>C5:14/DN is the address for counter file 5, counter 14’s done bit.</li>
<li>C5:14/CU is the address for counter file 5, counter 14’s count-up-enable bit.</li>
<li>C5:14/EN is the address for counter file 5, counter 14’s enable bit.</li>
</ul>

#### Working of a counter :
<p style="text-align: justify;">A counter instruction is always an output instruction. The counter instruction counts each time the input logic changes the rung state from false to true. This input logic can be signal from an external device, for e.g. limit switch or sensor, or a signal from internal logic. Every time the counter instruction sees a false-to-true rung transition, a count-up counters accumulated value is incremented by one.
The working of down-counter is little different. Each time when count-down counter sees a false-to-true rung transition, its accumulated value is decremented by one. Since the accumulated value gets decremented by 1 when each time the input logic changes the rung state from false to true, the accumulated value must be chosen as the starting point of the count.
<br><br>Counters are retentive in nature. The counter will retain its accumulated value or the on or off status of the done, overflow and underflow bits through a power loss.
</p>

### The count-up instruction :
<center>
<img src="images/counter/5.gif" style="width:536px; height:250px;"></center>

<p style="text-align: justify;">
The count-up instruction is used if we want a counter to increment one decimal value each time it register a rung transition from false to true. Each time input I:1/0 has a transition from off to on, counter C5:1 will increment its accumulated value by one decimal value. The count-up-enabled bit, on rung 001 is set when the rung conditions are true, or enabled. In rung 002, the done bit, DN, is set when the accumulated value is equal to or greater than the preset value. In the event of wrap from +32,767 to -32,768, the accumulated value becomes less than the preset value and the done bit will not be reset. In rung 003, the count-up overflow bit, OV, is set whenever the count-up counters accumulated value wraps from +32,767 to -32,768.</p>

### The count-down instruction :
<p style="text-align: justify;">This instruction is used when we want to count down over the range of +32,767 to -32,768. Accumulated value will be decremented by one count, each time the instruction sees a false-to-true transition. Count-down instruction has many applications, for example: if we want to display the remaining number of parts to be filled for a specific order say 50 parts, then, a count-down instruction can be used. In this example, the accumulated value will be set as 50 and the preset value will be 0. Each time a part is completed and passes the sensor, the accumulated value will be decremented by one decimal value, as shown in the figure below.</p>

<center>
<img src="images/counter/6.gif" style="width:578px; height:539px;"></center>
</p>

#### Ladder explanation :
<p style="text-align: justify;">Here, the sensor which sends the signal to the PLC when each time the part is completed and passes the sensor is taken from input screw terminal 0. It is used as a normally open instruction (-| |-) in rung 000       ( I:1/0 ). Accumulated value for the count-down counter C5:0 is set as 50. The preset value is 0. Each time input I:1/0 transitions from off to on, the accumulated value will be decremented by one decimal value. The done bit will true during the entire count from 50 to 0. In both rung 001 and 002 the done bit is used. In rung 001, done bit is used as normally open instruction (-| |-). The output screw terminal 0 is a lamp used to display the status of the process (not completed state). As the done bit will be true during the entire count from 50 to 0, the logical continuity will be established in rung 000 during the entire count from 50 to 0 and not completed lamp ( O:2/0 ) will be glowing. In rung 002, the done bit is used as normally closed instruction (-| |-). The output screw terminal 1 is a lamp used to display the status of the process (completed state). So when the accumulated value decreases and becomes equal to preset value (in this case 0), done bit of the counter is reset. So in rung 002, logical continuity is established and completed lamp (O:2/1)  becomes energized. In other word our process completed status is displayed. Rung 004 is used to reset the counter. Resetting the counter will cause the accumulated value to be reset to 0. Our application specifies that we count down from 50 to 0. After each completed cycle and counter reset, the accumulated value is reset to 0. In order to replace the accumulated value of 50 back into the counter’s accumulator, the move instruction on rung 003 is used. The move source in integer 50. The move instruction’s destination for the integer 50 is C5:0.ACC. Now, our counter is set to count down from 50 to 0.</p>

#### High - speed counter instruction (HSC) :
<p style="text-align: justify;">Few programmable logical controls have a special feature called as high-speed counter instruction. In Allan Bradley, the high speed counter- instruction is only used with the SLC 500 fixed PLC’s. If we are using modular SLC 500 chassis and a modular processor, then a separate high- speed counter module is used. </p>

<center>
<img src="images/counter/7.gif" style="width:523px; height:182px;"></center><br>
The above figure shows a high speed counter instruction of a fixed PLC (SLC micrologic 1000). Normally one high speed counter is allowed per program.  The high speed counter can be programmed as either an up-counter or a bidirectional counter. In the above figure, it is programmed as an up-counter. The counter used is somewhat similar to the CTU. However, high-speed counter is only enabled by the rung on which it resides. The high speed counter instruction does not count rung instructions. These transitions are counted from input I:0/1.
</p>

##### Associated status bits :
<p style="text-align: justify;">In this instruction, instead of counting the rung transition, the transitions seen at the input point is counted. As a result, many counts may come in between program scans and could be lost, resulting in an inaccurate accumulated value. So in order to solve this problem and provide an accurate update of the accumulated value, a separate hardware accumulator is used in conjunction with the high-speed counter. In word zero, status bit 10, is the update accumulator (UA) bit. Setting the UA bit will cause the instruction image accumulator (in the hardware accumulator used with HSC) to update the processors accumulator.</p><br/>

##### How to reset a counter instruction?
<p style="text-align: justify;">Reset instruction is used to reset a count-up counter, or count-down counter’s accumulated value to zero. This instruction also works for timers. The address of the reset instruction must match the address of the counter or the timer that is to be reset. Only one address is allowed per reset instruction.<br>
<center>
<img src="images/counter/8.gif" style="width:465px; height:294px;"></center><br>
The figure above shows a reset instruction on rung 001, which will reset counter C5:0 on rung 000.When the instruction I:1/1 becomes true, C5:0 counter is reset. When resetting a timer or counter instruction, accumulated values and status bits are zeroed.</p>
