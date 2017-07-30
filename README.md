# PID controller for autonomous driving


[video1]: data/1 0 0.mp4 "Start the manual tuning "
[video2]: data/0.5 0 0.mp4 "Half the Kp parameter due to oscillation"

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

### Paraeter tuning
In this work, manual project tuning was done as the efforts to utilize automatic techniques ran into issues when the car went out of track. The methodology to find parameters was as follows:

* First start by setting the I and D parameter values to zero, and P to 1. This resulted in the car oscilling as can be seen in the following [video](video1)
