# Calibrating Your Micromouse

## Sensor Calibration

### Reading Sensor Values

Here is an example of a sensor readings line:

~~~sh
|   158    29    23   144  |     97    10     8    94  |   189    3  |     69     | 
~~~

The first 4 readings are raw, direct from the ADC.  Layout is as per the sensor board,
so first reading is left-front, then left-side, right-side, right-front.

The next 4 readings are normalized and made more linear.

The next pair of readings are sum and difference of the front sensors

The final number is a "distance" in mm to the wall ahead

### Side Sensor Calibration

RAW side sensor values when robot is centred in a cell and no wall ahead

~~~sh
##                            ##
||                            ||
||                            ||
||          ______            ||
||         /       \          ||
||         |       |          ||
||         |||   |||          ||
||         |       |          ||
||         |_______|          ||
##============================##
~~~

Read the sensor values using `F 1` for continual read of sensors, or `S` for a single sensor reading

~~~cpp
// RAW values for the side sensors when the robot is centered in a cell
// and there is no wall ahead
const int LEFT_CALIBRATION  = 289;
const int RIGHT_CALIBRATION = 282;
~~~

### Front Sensor Calibration

RAW values for the front sensor when the robot is backed up to a wall

~~~sh
##=============================##


             ______
           /       \
           |       |
           |||   |||
           |       |
           |_______|
##=============================##
*/
~~~

The `FRONT_LEFT_CALIBRATION` and the `FRONT_RIGHT_CALIBRATION` are just the raw readings
when the robot is backed up against the wall as the diagram above

~~~cpp
const int FRONT_LEFT_CALIBRATION = 162;
const int FRONT_RIGHT_CALIBRATION = 152;
~~~

The front linear constant is the value of k needed to make the function
sensors.get_distance(sensor,k) return 68 when the mouse is backed up
against a wall with only a wall ahead.

The function `get_distance()` in `sensors.h` is as follows:

~~~cpp
  float get_distance(float sensor_value, float k) {
    float distance = k / sqrtf(sensor_value);
    return min(200, distance);
  }
~~~

So k is linear in the production of the distance, so if your distance reads
34 then the linear constant needs to be doubled so that the distance reads 68.
Do some math for the not-so-easy examples :-)

~~~cpp
const int FRONT_LINEAR_CONSTANT = 950;
~~~

`FRONT_REFERENCE` is pretty straightforward as per code comments

~~~cpp
const int FRONT_REFERENCE = 190;  // front sum reading when mouse centered with wall ahead
~~~

