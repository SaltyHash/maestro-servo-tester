# maestro-servo-tester
Turns a [Pololu Maestro](https://www.pololu.com/category/102/maestro-usb-servo-controllers) into a servo tester.

The script and settings in the file `mini_maestro_12_settings.txt` were created for the [Pololu Mini Maestro 12](https://www.pololu.com/product/1352), but the script should be portable across the other Maestro models as well.

To import the settings into your Mini Maestro 12: Run the Maestro Control Center, connect to your Maestro, click `File > Open settings file...`, and choose the settings file.

As currently written, the script will use channels 0..5 as servo outputs, and channels 6..11 as analog inputs.  Servo 0 is controlled by input 6; servo 1 is controlled by input 7; etc.

Each input is meant to be connected to the output of a linear potentiometer wired as a 0..5V voltage divider -- I recommend using the 5V output of the Maestro's built-in regulator as the voltage source.  Wire the potentiometers such that they output 0V at full CCW, and 5V at full CW.

**Note:** The script is currently written such that if an input sees 0V, it will actually *center* the servo (1.5ms pulse), rather than setting it to full CCW (1ms pulse).

Here is the script:
```
begin
  0  6 set_position
  1  7 set_position
  2  8 set_position
  3  9 set_position
  4 10 set_position
  5 11 set_position
repeat

# Args: <servo> <input>
sub set_position
  get_position

  # If pot is at 0, center servo; otherwise, interpolate
  dup
  0 equals
  if
    drop 6000
  else
    4 times 4000 plus
  endif

  swap
  servo

  return
```
