ECE382_Lab05
============

MSP430 IR remote reciever

###Pre-Lab
1) How long will it take the timer to roll over?

TACCR0 = 0xFFFF

TACTL = ID_3

0xFFFF µs/rollover


2) How long does each timer count last?

1 us



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


###Lab Functionality
The transition to functionality started with the start5.c file which contained the outline of the code pictured below

![alt tag](http://ece382.com/labs/lab5/schematic.jpg)

The init funciton was already complete so I began with the interrupts. 

#####Part2 Vector
First to check if it was rising or falling edge the switch on pin was created. Case 0 is falling edge and Case  1 is rising edge.

```C
#pragma vector = PORT2_VECTOR			// This is from the MSP430G2553.h file

__interrupt void pinChange (void) {

	int8	pin;
	int16	pulseDuration;			// The timer is 16-bits

	if (IR_PIN)		pin=1;	else pin=0;

	switch (pin) {					// read the current pin level
		case 0:						// !!!!!!!!!NEGATIVE EDGE!!!!!!!!!!
			pulseDuration = TAR;
			if((pulseDuration < maxLogic0Pulse) && (pulseDuration > minLogic0Pulse)){
				irPacket = (irPacket << 1) | 0;
			}
			if((pulseDuration < maxLogic1Pulse) && (pulseDuration > minLogic1Pulse)){
				irPacket = (irPacket << 1) | 1;
			}
			packetData[packetIndex++] = pulseDuration;
			TACTL = 0;				//turn off timer A e.w.
			LOW_2_HIGH; 				// Setup pin interrupr on positive edge
			break;

		case 1:							// !!!!!!!!POSITIVE EDGE!!!!!!!!!!!
			TAR = 0x0000;						// time measurements are based at time 0
			TA0CCR0 = 0x2710;
			TACTL = ID_3 | TASSEL_2 | MC_1 | TAIE;
			HIGH_2_LOW; 						// Setup pin interrupr on positive edge
			break;
	} // end switch

	P2IFG &= ~BIT6;			// Clear the interrupt flag to prevent immediate ISR re-entry

} // end pinChange ISR
```
Case 1: The most important aspec tof case 1 was building the IR data packet. This was done using known values for length of 1s and 0s from a given remote.

Case 2: On the positive edge, the timers and interrups had to be properly set. TACCR0 was calculated for the proper rollover time. 

#####Timer A Vector
The timer interrupt was a bit similar to the rising edge of the hardware interrupt in the sense that it managed settings of timers and flags. The following code accomplished what is in the picture under TimerA

```C
#pragma vector = TIMER0_A1_VECTOR			// This is from the MSP430G2553.h file
__interrupt void timerOverflow (void) {

	TACTL = 0;
	TACTL ^= TAIE;
	packetIndex = 0;
	newPacket = TRUE;
	TACTL &= ~TAIFG;
}
```

