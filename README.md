ECE382_Lab05
============

MSP430 IR remote reciever

###Pre-Lab
1) How long will it take the timer to roll over?
  TACCR0 = 0xFFFF
  TACTL = ID_3

   0xFFFFCnts     8 clks       1 µs
  ------------ x --------- x -------- = 0xFFFF µs/rollover
   1 rollover     1 Cnt       8 clks
 
 2) How long does each timer count last?
  
