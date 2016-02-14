---
layout: post
title: How To Wire A Cake
tags:
- cosplay
- costume_electronics
- homestuck
---

When Jake decided to go to GenCon as Dad Egbert, he bought a string of grain-of-wheat LEDs to light up his prop cake.  I suggested going one step further: I'd make them flicker like real candles.

<img src="/images/cake/dad john cake 1.jpg" class="blogpost-center" />

Plan 1: LED flashers
----

<img src="/images/cake/Cake-flasher.png" class="blogpost-center" />

555 timer chips are great for making LEDs blink.  They're cheap and reliable, and you can change the blink speed by varying a resistor (R11, in the diagram above).

Because pin 3, the output, alternately sinks and sources power, you can do more than just turn things on and off.  You can connect one set of LEDs (or whatever) to Pin 3 using the cathodes, and another set using the anodes, so that one group is on when the other is off.  And the 555 will deliver up to 200mA, so you could have about ten typical LEDs powered at one time before you have to start driving them through a transistor or the like.

By alternating LEDs from the two groups, you can get an effect that suggests flickering candles.  It's even more effective when the individual LEDs are separated a bit, the way they would be on the cake, since it's harder to see that they're in two groups, and that they go completely on and off.

<!-- video of blinking LEDs -->
<iframe width="560" height="315" src="https://www.youtube.com/embed/0EIpySjNWlM" frameborder="0" allowfullscreen></iframe>

However, I'd forgotten about the grain of wheat lights when I breadboarded this up.  All the LEDs on the string light up at once – you'd need two strands to get an alternating effect.  With just one strand, it's much more visible that the lights are switching off with a regular pattern.

Plan 2: Throbbing LEDs
----

What broke the illusion with the single strand of lights was seeing them switch off altogether.  But what if they faded on and off?

<img src="/images/cake/Cake - throbbing.png" class="blogpost-center" />

There are a number of pulsing/fading/throbbing LED circuit diagrams out 
there that fade the LEDs on and off by charging and discharging a capacitor.  
I've had good results from the one in this [All About Circuits article on 
LEDs](http://forum.allaboutcircuits.com/threads/leds-555s-flashers-and-light-chasers.19075/), which also discusses in detail just about anything else you
might decide to do with them.  You can change how often the LEDs pulse by 
turning the potentiometer.  There are other LED throbber circuit designs that 
don't use transistors, but the ones I've tried tend to be finicky – they 
didn't always work as written, though sometimes adjusting the resistor and
capacitor values help.

<!-- video of throbbing lights -->
<iframe width="560" height="315" src="https://www.youtube.com/embed/3NYJ0mvF0Og" frameborder="0" allowfullscreen></iframe>

Speaking of finicky, while that throbbing circuit worked just fine when I plugged in individual LEDs (even in parallel), I just couldn't seem to get a similar fading effect when I put in the grain-of-wheat lights.  They just blinked on and off, no matter how I tweaked the other components.

Plan 3: Fix It In Software
----

So instead of trying to build a circuit that automatically did what I wanted, I built a simple circuit and programmed a microcontroller to tell it what I wanted.

The ATtiny13a is an eight-pin microcontroller that'll run you under forty cents on eBay if you buy a pack of ten.  It's made by Atmel, who also make the ATmegas that drive Arduinos, and the ATtiny13a is among their teeniest microcontrollers.

Programming the ATtiny13a was maddening.  There are a zillion online tutorials, none of which worked for me from beginning to end.  (Perhaps each one omitted some step the author thought was obvious.  If so, I've contributed to the problem with this post.)  Here's what finally got me up and running.



### 1. The easy part: set up an Arduino Uno as an programmer

You can program an Arduino by plugging it into your computer's USB port.  How do you program an integrated circuit?

I already had an Arduino Uno, so I set it up as an in-system programmer (ISP).  Just connect your Uno to your USB port, and choose this option from the Arduino GUI:

<img src="/images/cake/arduino as isp.png" class="blogpost-center" />

### 2. The harder part: set up boards.txt

Strictly speaking, setting up boards.txt is easy.  Finding appropriate entries for the ATtiny13 is hard.

The Arduino software (not just the GUI, but avrdude as well) checks the boards.txt files for information about the board or integrated circuit.  Discouragingly, even very experienced Arduino hackers gripe about the poor documentation for the boards.txt format – and some of the boards files out there didn't work for me.

One of them that did work was [smeezekitty's boards.txt]( http://sourceforge.net/projects/ard-core13/?source=typ_redirect) – you can download a zipfile at the link.

To install it:

1.	 Close the Arduino software if it's open.
2.	 Your *My Documents/Arduino* folder probably has a *hardware* folder already, but if it doesn't, create one.
3.	Unzip the zip file and put the attiny13 folder in the *My Documents/Arduino/hardware* folder.
4.	Open your Arduino GUI and go to Tools | Boards.  You should see entries for the ATtiny13.  (We won't actually be using the GUI to program the ATtiny, but this is an easy way to confirm that your setup is working.)

### 3. Hook up the Arduino Uno to the ATtiny13a

On the Uno, add a 10 uF capacitor **between reset and ground**.  Then put the ATtiny13a on a breadboard and connect the pins as follows:

<table class="blogpost">
<thead>
<tr>
  <th>Arduino</th>
  <th>ATtiny13a</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5V</td>
  <td>Pin 8</td>
</tr>
<tr>
  <td>Ground</td>
  <td>Pin 4</td>
</tr>
<tr>
  <td>Pin 10</td>
  <td>Pin 1</td>
</tr>
<tr>
  <td>Pin 11</td>
  <td>Pin 5</td>
</tr>
<tr>
  <td>Pin 12</td>
  <td>Pin 6</td>
</tr>
<tr>
  <td>Pin 13</td>
  <td>Pin 7</td>
</tr>
</tbody>
</table>

<img src="/images/cake/Arduino Uno ISP schematic.png" class="blogpost-center" />

### 4.  Getting code – and getting it onto the ATtiny

The ATtiny13a only has 1K of program memory, which is plenty for pseudo-randomly flickering an LED.  smeezekitty has written an [ATtiny13 core]( http://forum.arduino.cc/index.php?topic=89781.0) that lets you write – within that 1K limitation – code that looks like what you're used to for the regular Arduino.  

After quite a few iterations of trying to pare down a simple flickering-light program for the Uno so I could cram it onto the ATtiny13a, though, I decided to look at someone else's code.

Mark VandeWettering created [an ATtiny13-controlled jack-o'-lantern](http://brainwagon.org/making-an-attiny13-powered-pumpkin/) that almost did what I had in mind.  His program can either fade the LED off and on (like the throbbing circuit above) or flicker it pseudo-randomly, but defaults to throbbing.  Rather than leave the program as-is and jumper the input pin to the right value, I just cut out the throbbing code.

<pre><code>
#define F_CPU 1000000UL
#include <avr/io.h>
#include <util/delay.h>

/*
 * flicker.c
 *
 * Based on Mark VandeWettering's more complicated candle.c
 * code from http://brainwagon.org/making-an-attiny13-powered-pumpkin/ 
 * 
 * Flicker an LED on Pin 5 of an ATtiny13 randomly.
 *
 * Written by Mark VandeWettering for Halloween, 2011
 *
 * Compile using avr-gcc and the following commands:
 *   avr-gcc -g -Os -c -mmcu=attiny13 flicker.c
 *   avr-gcc -mmcu=attiny13 candle.o -o flicker.elf
 *   avr-objcopy -O ihex -R .eeprom flicker.elf flicker.hex
 */
 
#define LED     PB0             /* pin 5 on the ATtiny13 */
	
int i;
	
unsigned int lfsr = 1 ;
	
signed char dir = 1 ;
	
int
main(void)
{
    /* LED is an output... */
    DDRB |= (1 << LED) ;
 		
    TCCR0A |= ((1 << COM0A1) | (1 << COM0A0) | (1 << WGM01) | (1 << WGM00)) ;
    TCCR0B |= (1 << CS01) ;
    OCR0A = 0;
 		
    for (;;) {
       	lfsr = (lfsr >> 1) ^ (-(lfsr&1u) & 0xB400u) ;
        OCR0A = (lfsr >> 8) ;
        _delay_ms(12);
	}
}
</code></pre>

If you've saved the code in a file called flicker.c, you can compile it into the format the ATtiny13a expects with the following commands:

<pre><code>
avr-gcc -g -Os -c -mmcu=attiny13 flicker.c
avr-gcc -mmcu=attiny13 candle.o -o flicker.elf
avr-objcopy -O ihex -R .eeprom flicker.elf flicker.hex
</pre></code>

Then - with your Arduino Uno plugged into your computer, and wired to the ATtiny13a as shown above - run the following command to copy the hext file to the ATtiny.  (If your Uno isn't on COM3, you can change the <code>-PCOM3</code> parameter to point to some other com port.  It's okay.)

<code>avrdude -C</code><em>path/to/your/config/file</em><code>\avrdude.conf" -v -pattiny13 -cstk500v1 -PCOM3 -b19200 -e -Uflash:w:flicker.hex</code>

My avrdude.conf is in the *hardware/tools/avr/etc* directory under my Arduino installation.  There is no point in asking me where yours is.  I don't know.

Since the grain-of-wheat LEDs were 4.5V, I used four AA batteries (to allow 
for the voltage drop from the microcontroller).  That's a little above the
maximum operating voltage for the ATtiny - which tops out at 5.5V - but in practice that's about what I was getting out of the rechargeables.  Single LEDs are 
usually a little above or below 2V, so if that's what you're connecting up, you could just use two AAs; it worked just fine when I tried it.  Your resistor value will depend on the voltage of your LED; there are [online LED resistor
calculators](http://led.linear1.org/1led.wiz) that'll help work out the right values.

I also added a rocker switch to turn the candles on and off without needing to unsnap the batteries.  Everything's soldered to a little piece of scrap perfboard.  

<img src="/images/cake/Cake - ATtiny13a.png" class="blogpost-center" />

There you go - one fully-lit, low-carb, gluten-free, convention-ready prop cake ready to fulfill all your dotesmiting needs!

<iframe width="560" height="315" src="https://www.youtube.com/embed/l3PsYJjILUU" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HJvukNBGGos" frameborder="0" allowfullscreen></iframe>

Materials
-----

**Tools:**

- Soldering iron & solder
- Arduino Uno (used as an ISP, not incorporated into the cake)

**For the wiring:**

- Perfboard
- ATtiny13a
- LEDs
- Resistor (exact value [depends on your LED](http://led.linear1.org/1led.wiz))
- Wire
- Rocker (on/off) switch
- Battery holder for 4 AA batteries (or some other number if you changed the LED)
- Snap connector for battery holder (if it didn't come with one)
- 4 AA batteries (did you change the LED? yes? then this might change too)


<div class="container">
<div class="row">
    <div class="col-sm-6 col-md-6 col-lg-6">
        <a href=""/images/cake/dad john cake 2.jpg">
        <img src="/images/cake/dad john cake 2.jpg" class="blogpost-grid">
        </a>
    </div>
    <div class="col-sm-6 col-md-6 col-lg-6">
        <a href="/images/cake/dad john cake 3.jpg">
        <img src="/images/cake/dad john cake 3.jpg" class="blogpost-grid">
        </a>
    </div>
</div>
<div class="row">
    <div class="col-sm-6 col-md-6 col-lg-6">
        <a href=""/images/cake/dad john cake 4.jpg">
        <img src="/images/cake/dad john cake 4.jpg" class="blogpost-grid">
        </a>
    </div>
    <div class="col-sm-6 col-md-6 col-lg-6">
        <a href="/images/cake/dad john cake 5.jpg">
        <img src="/images/cake/dad john cake 5.jpg" class="blogpost-grid">
        </a>
    </div>
</div>
</div>
