# Read Me for Hardware PWM
## Author: Ardit Pranvoku
To run this code, simply import the project folder into code composer, build, then debug.
Uses the timer to generate a signal that will cause an LED to blink at a designated PWM.
The watchdog timer must be stopped with the line WDTCTL = WDTPW + WDTHOLD or WDTCTL = WDTPW | WDTHOLD.
By using the line PM5CTL0 = ~LOCKLPM5, the default high impedance on the board is disabled.
Else, the processor will reset.
The desired led pins and bits must be set to 1 to configure it to be an output.
The desired button pin and bit must be to 0 to configure it to be an input.
Also,  PXREN |= BITX; must be used to enable the pullup resistor for that button.     
This high impedance serves to get rid of any cross currents, but is turned off later.

TA0CTL is configured using the desired settings. We used Timer A, SMCLK, up mode, and clear TAR so our code was:
TA0CTL = TASSEL_2 + MC_1 + TACLR;
We also set TA0CCTL to OUTMOD_7 so the timer out mode is in reset/set.

TA0CCR0 is initialized at 99.
TA0CCR1 is initialized at 50, so the duty cycle is 50%. This is because the LED is set at 99(CCR0) and reset at 50,
(CCR1) therefore it's on half the time. 

P1SEL0 |= BIT0; and P1SEL1 &= ~BIT0;
 directs the signal of the timer to LED 1.0.

The program uses the a button press to change TA0CCR1.
While the button is pressed, TA0CCR1 is increased by 10, adding 10% to the duty cycle. The CPU waits for 
100,000 cycles before incrementing again.
At 100% duty cycle, TA0CCR1 is reset to 0. 