# Project 05 Term 2 - Model Predictive Controller Project - Control actuators of the vehicle.
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

## README
The purpose of this project was to "build a MPC controller and modify the actuators to reduce CTE," and to "test your solution on the simulator!" The simulator provides cross-track error (CTE), speed, and steering angle data via local websocket. The MPC controller must be robust to 100ms latency, as one may encounter in real-world application.

As described in the lessions IPOPT and CPPAD libraries are used to find an optimal trajectory and its associated actuation commands in order to minimize error with a third-degree polynomial fit to the given waypoints. It considers very short duration of worth of waypoints and trajectory for that duration based upon a model of the vehicle's kinematics and a cost function based mostly on the vehicle's cross-track error (roughly the distance from the track waypoints) and orientation angle error, with other cost factors included to improve performance.

## RUNNING THE CODE

Once you have this repository on your machine, cd into the repository's root directory and run the following commands from the command line:
```
> mkdir build
> cd build
> cmake ..
> make
> ./mpc
```
> **NOTE**
> If you get any `command not found` problems, you will have to install 
> the associated dependencies (for example, 
> [cmake](https://cmake.org/install/))

You have to run Term 2 simulator (Project 5) and you can see the results in the simulator.

### Model discussion
The kinematic model includes the vehicle's x and y coordinates, orientation angle (psi), and velocity, as well as the cross-track error and psi error (epsi). Actuator outputs are acceleration and delta (steering angle). The model combines the state and actuations from the previous timestep to calculate the state for the current timestep based on the equations below:

![equations](./eqns.png)

### Parameters Selection
The values chosen for N and dt are 10 and 0.1, respectively. Values are tuned by manually selecting different combinations like 20 and 0.2, 10 and 0.5. After trying different combinations found best results on 10 and 0.1.

**Polynomial Fitting and MPC Preprocessing**
The waypoints are preprocessed by transforming them to the vehicle's perspective (see main.cpp lines 108-113). This simplifies the process to fit a polynomial to the waypoints because the vehicle's x and y coordinates are now at the origin (0, 0) and the orientation angle is also zero. 

**Model Predictive Control with Latency**
100 ms latency is handled in two ways.
1. Rather than choosing t-1 state of actuators, I choose t-2 state so that t-1 state is always current state of actuators.
2. Added penalty for steering angle and velocity on cross track error so that car always take smooth turns.

Also, as suggested in the lessons of punishing CTE, epsi, difference between velocity and a reference velocity, delta, acceleration, change in delta, and change in acceleration are added in  the cost functions.

            


