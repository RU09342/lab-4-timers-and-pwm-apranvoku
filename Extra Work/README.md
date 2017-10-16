# Read Me for Extra Work
## Author: Ardit Pranvoku
To run this code, simply import the project folder into code composer, build, then debug.
Uses the timer to generate a signal that will cause an LED to blink at a designated PWM.
Uses logarithms to account so the PWM is easier to see with the human eye.
The watchdog timer must be stopped with the line WDTCTL = WDTPW + WDTHOLD or WDTCTL = WDTPW | WDTHOLD.
By using the line PM5CTL0 = ~LOCKLPM5, the default high impedance on the board is disabled.
Else, the processor will reset.

The desired led pins and bits must be set to 1 to configure it to be an output.
The desired button pin and bit must be to 0 to configure it to be an input.
Also,  PXREN |= BITX; must be used to enable the pullup resistor for that button.     


TA0CTL is configured using the desired settings. We used Timer A, SMCLK, up mode, and clear TAR so our code was:
TA0CTL = TASSEL_2 + MC_1 + TACLR;
We also set TA0CCTL to OUTMOD_7 so the timer out mode is in reset/set.

P1SEL0 |= BIT0; and P1SEL1 &= ~BIT0;
 directs the signal of the timer to LED 1.0.

The program uses a button press to change TA0CCR1 logmarithically.
taps is initialized to 10.
While the button is pressed, taps is decreased by 1.
logNum is then set to log10(taps) * 100, which will start off at log10(9) * 100 and decrease
every loop until it ends at 0.
incrementNum is set to 100 - logNum, so it will start off at 0 and decrease every loop until it ends at
at 100. 
Finally, TA0CCR1 is set equal to incrementNum, so it will behave similarly.
This means that that button will start dim and increase logmarithically to 100. 

The CPU will always wait 100,000 cycles before incrementing again.
At 100% duty cycle, taps is reset back to 10.