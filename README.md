ECE382_Lab05
============

MSP430 IR remote reciever

###Pre-Lab
1) How long will it take the timer to roll over?

TACCR0 = 0xFFFF

TACTL = ID_3

0xFFFF µs/rollover


2) How long does each timer count last?



<table class="table table-striped table-bordered">
<thead>
<tr>
<th align="center">Pulse</th>
<th align="center">Duration (ms)</th>
<th align="center">Timer A counts</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="1">Start logic 0 half-pulse</td>
<td align="center" colspan="1">8.9556 ms</td>
<td align="center" colspan="1">8888</td>
</tr>
<tr>
<td align="center" colspan="1">Start logic 1 half-pulse</td>
<td align="center" colspan="1">4.4498 ms</td>
<td align="center" colspan="1">4412</td>
</tr>
<tr>
<td align="center" colspan="1">Data 1 logic 0 half-pulse</td>
<td align="center" colspan="1">592.8 us</td>
<td align="center" colspan="1">591</td>
</tr>
<tr>
<td align="center" colspan="1">Data 1 logic 1 half-pulse</td>
<td align="center" colspan="1">1.6312 ms</td>
<td align="center" colspan="1">1616</td>
</tr>
<tr>
<td align="center" colspan="1">Data 0 logic 0 half-pulse</td>
<td align="center" colspan="1">625.2 us</td>
<td align="center" colspan="1">620</td>
</tr>
<tr>
<td align="center" colspan="1">Data 0 logic 1 half-pulse</td>
<td align="center" colspan="1">474.6 us</td>
<td align="center" colspan="1">468</td>
</tr>
<tr>
<td align="center" colspan="1">Stop logic 0 half-pulse</td>
<td align="center" colspan="1">600 us</td>
<td align="center" colspan="1">593</td>
</tr>
<tr>
<td align="center" colspan="1">Stop logic 1 half-pulse</td>
<td align="center" colspan="1">40 ms</td>
<td align="center" colspan="1">39263</td>
</tr>
</tbody>
</table>

<table class="table table-striped table-bordered">
<thead>
<tr>
<th align="center">Button</th>
<th align="center">code (not including start and stop bits)</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" colspan="1">0</td>
<td align="center" colspan="1">00000000111111111101100000100111 0xffd827</td>
</tr>
<tr>
<td align="center" colspan="1">1</td>
<td align="center" colspan="1">00000000111111110000100011110111 0xff08f7</td>
</tr>
<tr>
<td align="center" colspan="1">2</td>
<td align="center" colspan="1">00000000111111111100000000111111 0xffc03f</td>
</tr>
<tr>
<td align="center" colspan="1">3</td>
<td align="center" colspan="1">00000000111111111000000001111111 0xff8074</td>
</tr>
<tr>
<td align="center" colspan="1">Enter</td>
<td align="center" colspan="1">00000000111111111010000001011111 0xffa05f</td>
</tr>
</tbody>
</table>
