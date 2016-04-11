It is possible to access the baseband even while it is in use by CommCenter. To do this, simply use minicom and set the serial port to /dev/tty.debug, instead of /dev/tty.baseband.

The downside of this trick is that it is very slow. You can push your spacebar down until it starts responding. Once it starts responding, enter any commands you want. You can even dial a number by using `ATDT number;` (the `;` on the end is very important).

# Possible Uses #

Some possible uses are:

  1. Manipulating the baseband in parallel with CommCenter
  1. Enabling trace and debug to see what the iPhone does
  1. Redirect /dev/tty.debug on a Bluetooth connection