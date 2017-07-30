# PID controller for autonomous driving
[video1]: data/1_0_0.mp4 "Start of the parameter tuning"
[video2]: data/05_0_0.mp4 "Half the Kp parameter due to oscillation"
[video3]: data/0125_0_0.mp4 "Final Kp parameter"
[video4]: data/015_0_1.mp4 "Start tuning Kd"
[video5]: data/015_00004_4.mp4 "Final result"
[video6]: data/008_0001_6.mp4 "50mph"

In this project, a PID controller was desigend to automatically keep a simulated car (from [here](https://github.com/udacity/self-driving-car-sim/releases) ) driving as close to the center of track as possible. In order to do so, hyper paramters for the p, i, and d componments of the PID controller were tuned manually. 

## Building
To build:
* mkdir/cd build
* cmake ..
* make
* run `./pid <args>` (where args are the Kp, Ki, and Kd parameters respectively, e.g. `./pid 0.15 0.0001 4`
* start the simulator found from [here](https://github.com/udacity/self-driving-car-sim/releases)

## Discussion
### Effect of different gain hyper parameters
#### P (proportional)
The P paratemer is the main error correcting parameter in the PID controller. The cross-track-error (cte) is multiplied by this parameter, higher `Kp` values result in a faster responce time, but also oscillations around the middle of the track. 

#### D (differential)
This parameter is in charge of tracking abrubt changes, and is affects the derivative (in this case the difference between current cte and the previous cte). By penalyzing having different ctes in subsequent time instances, we help prevent overshooting and oscillations around the center of the track and approach in a more smoother fashion

#### I (integral)
This parameter tracks the total error in previous time instances. By tracking previous errors, it enables us to remove any possible biases when steering, for example when we think we are steering in a straight line but due to some errors we are actually not driving in a straight line.

### Parameter tuning
In this work, manual project tuning was done as the efforts to utilize automatic techniques ran into issues when the car went out of track. The methodology to find parameters was as follows:

* First start by setting the I and D parameter values to zero, and P to 1. This resulted in the car oscillating, as can be seen in the following [video][video1]
* Next the `Kp` parameter is halved, and tested again (as seen [here][video2]). This process is done until the car seems to be able to drive straight along the (non curved) path. The result is `Kp` ~ 0.125 [as seen here][video3]
* After setting the `Kp` parameter, we can see that it still oscillates around curves, the `Kd` term is increased to 1 [as seen here][video4]. This process is continued until the oscillations are acceptable.
* Finally, to help with sharp turns, the `Ki` parameter is increased slightly. This was done iteratively (staring from 0.0001) until the result was deemed satisfactory.
* After setting all three paraemters, further manual fine tuning was done to reduce oscillations and improve performance around corners.

The final result (when driving around at 30mph) is shown [here][video5], executed with `./pid 0.15 0.0004 4`.

## Higher speeds
Tests were also done by increasing the throttle value (line 78 in main.cpp) to 50mph, and it was noticed that driving is no longer safe with the previous hyper parameters. In order to be able to drive safely in this case, it was necessary to decrease the `Kp` parameter to avoid overshooting. In order to be able to stay close to the middle, the `Kd` and `Ki` parameters were increased (while normally `Ki` only tracks bias issues during the manual testing it was noticed that it helped on curves). The final result is shown [here][video6], executed with `./pid 0.08 0.001 6`.

Tests with higher speeds (70mph) did not result in a safe driving with the manual tests. Further testing to optimize the parameters (or using techniques such as twiddle) need to be done to be able to handle the higher speed. Further, adding a PID controller to control the speed could be the next step, as it will greatly help during quick turns.
