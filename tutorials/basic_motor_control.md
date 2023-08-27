---
layout: page
title: Basic Motor Control
---

Basic Motor Control
===================

The aim of this tutorial is to get a motor turning with your kit.
To complete this tutorial, you'll need the following:

* The [power board](/docs/kit/power_board)
* A battery (charged, of course)
* A [motor board](/docs/kit/motor_board)
* 2 large (7.5mm) green _CamCon_ connectors to plug the power board and motor board together
* 2 lengths of a suitable gauge of wire for powering the motor board from the power board (one should be black/grey)
* A motor (see specification below)
* A small (5mm) green _CamCon_ connector to plug the motor and motor board together
* 2 lengths of a suitable gauge of wire for your motor
* A Micro USB cable
* A USB hub
* A standard USB cable
* The memory stick
* A soldering Iron
* Some solder wire
* Wire strippers
* A small slotted/flat blade screwdriver (for the _CamCon_ screws)

You should be familiar with the setup for most of the above now, so it's just
the motor-related parts that need explaining.


Motor Specification
-------------------

There is a certain specification the motor(s) you use must meet.
The criteria are as follows:

* 12V motor
* A stall current of less than 10A (this is the current limit for the [motor boards](/docs/kit/motor_board))

<div class="info">
    When designing your robot you should bear in mind that while each motor board can deliver 10A on each output,
    all the power needs to go through the power board, which is limited to 30A overall.
    This means that across all the outputs for all the motors (as well as the rest of your kit),
    you can only draw up to 30A at any time.
</div>

Connecting a Motor
------------------

To plug the motor into the kit,
 you'll need to solder an appropriate gauge of wire to the terminals on the motor,
 and connect the other ends to the _CamCon_ connector.
Like so:

![Motor connected to CamCon]({{ site.baseurl }}/images/content/kit/motor_and_camcon.png)

<div class="info">You may want to insulate the motor's terminals with some insulation tape (or heat shrink tubing if you've got it) like in the image above.</div>

Now you need to connect the motor to one of your motor boards.
You'll need to connect the following:

* Your motor into the motor 0 socket on the motor board
* The micro USB cable from the motor board to the USB hub
* The standard USB cable from the USB hub to the power board

This is almost ready, but the motor board also needs the power that it will be delivering to the motor.
This is done by connecting together the two larger _CamCon_ connectors, using the other two lengths of wire.
Be sure that the cable connects the positive motor rail output ("+") of the power board to the positive power input of the motor board, and likewise for the ground ("-") output &mdash;
 see the [power board](/docs/kit/power_board) and [motor board](/docs/kit/motor_board)
 documentation to see which is which.

<div class="info">
    You must always use black or grey for ground (0V) connections (and only for these), so that it's clear where these are.
</div>

You can now connect this into the power board on one end, and the motor board power connection on the other.

Some Code
--------

<div class="warning">
    You might want to ensure the motor won't take the kit anywhere when you
    press the run button when testing some of the below code (unless it's in a robot, of course).
</div>

The example program we'll write will do a number of things with the motor:
forwards and backwards, and different power settings, for example.
Let's begin.

To start off with, we'll just make a motor move forwards, then backwards, and then repeat.


### Forwards & Backwards

Doing this is actually very easy; the only thing you need to realise is that a positive number is forwards and a negative number is backwards.

<div class="warning">
    The actual direction of travel of a motor, when mounted on a robot,
    will depend on its orientation and the way in which the wires are connected to the motor board.
    If the motor appears to be going in the wrong direction,
    just swap the motor's positive and negative wires over.
</div>

Here's the code:

~~~~~ python
from sr.robot3 import *
import time

R = Robot()

while True:

    R.motor_board.motors[0].power = 0.5
    time.sleep(3.0)

    R.motor_board.motors[0].power = 0
    time.sleep(1.4)

    R.motor_board.motors[0].power = -0.5
    time.sleep(1)

    R.motor_board.motors[0].power = 0
    time.sleep(4)
~~~~~

You're familiar with the first few lines; in fact, the only lines you may not be familiar with are the `R.motor_board...` lines.
For a comprehensive reference to the `motor` object, see the `sr.robot3` module's [Motors](/docs/programming/motors) page.
But, to summarise:

<div class="info" markdown="1">
  `R.motor_board.motors[0].power = x` will set the power of the motor connected to output 0 (the `motors[0]` part)
  on the first [motor board](/docs/kit/motor_board) (the `motor_board` part)
  plugged in to a USB hub to `x`, where `x` is a value between `-1` and `1`,
  inclusive &mdash; in other words: `-1` &le; `x` &le; `1`.
</div>

So, `R.motor_board.motors[0].power = 0.5` sets the target power of the motor connected to output 0 on the first [motor board](/docs/kit/motor_board)
 plugged in to a USB hub to 50% forwards (i.e. a duty-cycle of 0.5 forwards).

As you would expect, then, `R.motor_board.motors[0].power = -0.5` will put the this motor into reverse at 50% power.
`R.motor_board.motors[0].power = 0` will output no power to the motor and stop it.

So, if you put the above code on your robot,
 you should be able to see a motor spin forwards, stop, spin backwards, stop, and then repeat...

<div class="info" markdown="1">
  If you find that the motor doesn't turn when you run the above code,
  check that you've got all the cables connected to the right places,
  in particular note that the motor board has _two_ outputs.
</div>

### Changing the Speed

Now we're going to modify the program to vary the speed of the motor.
Our aim is to do the forwards and backwards bit (as above), but, before we loop round again,
 ramp the power up to 70%, then down to -70%, and then back to 0 (all in steps of 10%).

Here's the code:

~~~~~ python
from sr.robot3 import *
import time

R = Robot()

while True:

    R.motor_board.motors[0].power = 0.5
    time.sleep(3.0)

    R.motor_board.motors[0].power = 0
    time.sleep(1.4)

    R.motor_board.motors[0].power = -0.5
    time.sleep(1)

    R.motor_board.motors[0].power = 0
    time.sleep(4)

    # ^^ code from before ^^

    # power up to 70% (from 10%)
    for pwr in range(10, 80, 10):
        R.motor_board.motors[0].power = pwr / 100  # Convert from percentage
        time.sleep(0.1)

    # power down from 70% (to 10%)
    for pwr in range(70, 0, -10):
        R.motor_board.motors[0].power = pwr / 100  # Convert from percentage
        time.sleep(0.1)

    # set power to 0 for a second
    R.motor_board.motors[0].power
    time.sleep(1)

    # power "up" to -70% (from -10%)
    for pwr in range(-10, -80, -10):
        R.motor_board.motors[0].power = pwr / 100  # Convert from percentage
        time.sleep(0.1)

    # power "down" to -10% (from -70%)
    for pwr in range(-70, 0, 10):
        R.motor_board.motors[0].power = pwr / 100  # Convert from percentage
        time.sleep(0.1)

    # set power to 0 for a second
    R.motor_board.motors[0].power = 0
    time.sleep(1)

~~~~~

Again, as you've seen most of that before, it shouldn't be too difficult to get your head around.
The `for` loop may be new, however.

The [`for`](https://docs.python.org/tutorial/controlflow.html#for-statements)
 loop accepts anything that Python can iterate over to get multiple values, such as a `list` (a `list`, when `print`ed, appears in square brackets like so: `[1, 2, 3]`).

For a comprehensive introduction to to `list`s, have a look at [this WikiBooks article](https://en.wikibooks.org/wiki/Python_Programming/Lists).
The `for` loop will iterate over the `list` (i.e. take each element in turn)
 and make it available in the loop's body as the variable after the the `for` keyword.
Here's an example:

~~~~~ python
for i in [1, 2, 3]:
    print(i)
~~~~~

The above would output:

~~~~~ not-code
1
2
3
~~~~~

Then there's the [`range()`](https://docs.python.org/3/library/stdtypes.html#typesseq-range) object.

`range()` objects represent a sequence of numbers. They can be constructed with
one, two or three arguments to describe what the sequence of numbers should be.

If only one argument is provided then the sequence starts at `0` and stops just
before the given number. Otherwise the first argument is the number to start at,
the second argument is the number to stop before and the (optional) third
argument is how big the steps should be (by default steps are `1`).

So, based on this, `range(3)` would represent the sequence `0`, `1`, `2` because
it is shorthand for `range(0, 3, 1)`.

So, taking `range(10, 80, 10)`, for example, would output `10` as the first element,
 then `20, 30, ...` up until `x=10+i*10` for some `i` where `i` ensures `x < stop` (which, in this case, is `80`).
So, the `80` we've used could equally have been `77` or even `71` and the outputted sequence would still be `10`, `20`, `30`, `40`, `50`, `60`, `70`.

Putting all of that together should mean you understand the above code.
You might want to run the code on your kit to see if it does what you expect it to.


Next Steps
----------

From here, you should be able to make your robot move about in a controlled manner.
See if you can make your robot drive forwards to a given point, stop, turn around and then return to its starting point.

You might also like to see if you can make the larger code example above more concise by creating functions for the repetitive parts.
[This](https://www.tutorialspoint.com/python3/python_functions.htm) tutorial seems good if you're interested.
