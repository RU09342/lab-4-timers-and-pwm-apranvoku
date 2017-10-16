# Read Me for Debounce
## Author: Ardit Pranvoku

To run this code, simply import the project folder into code composer, build, then debug.

Uses the timer to generate a signal that will cause a button to be disabled for a certain amount of time to 
prevent the bouncing effect in a button.

The watchdog timer must be stopped with the line WDTCTL = WDTPW + WDTHOLD or WDTCTL = WDTPW | WDTHOLD.

By using the line PM5CTL0 = ~LOCKLPM5, the default high impedance on the board is disabled.

Else, the processor will reset.
The desired led pins and bits must be set to 1 to configure it to be an output.

The desired button pin and bit must be to 0 to configure it to be an input.

Also,  PXREN |= BITX; must be used to enable the pullup resistor for that button.
This high impedance serves to get rid of any cross currents, but is turned off later.


Interrupts on a button to keep track of a button being pressed.
PXIE |=BITX;
PXIES |=BITX;
PXIFG &=~(BITX);


When the button interrupt is caused:
TA0CTL is configured using the desired settings.
 We used Timer A, ACLK, up mode, 2 divider, and clear TAR so our code was:

TA0CTL = TASSEL_1 + MC_1 + ID_1;


TA0CCR0 is initialized at 2000, this determines how long the button's interrupt is disabled.


TA0CCTL1 = (CCIE); Enables Interrupts on Timer A.


After the button is pressed and the timer value reached 2000, an interrupt is caused and 
the button's interrupt will be enabled again.
