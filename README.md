[//]: # (Image References)

[image1]: ./Gif/P1_1.gif "P1_1"
[image2]: ./Gif/P05_1.gif "P05_1"
[image3]: ./Gif/P05_2.gif "P05_2"
[image4]: ./Gif/P05_3.gif "P05_3"

[image5]: ./Gif/PD_1.gif "PD_1"
[image6]: ./Gif/PD_2.gif "PD_2"
[image7]: ./Gif/PD_3.gif "PD_3"
[image8]: ./Gif/PD_4.gif "PD_4"

[image9]: ./Gif/PID_1.gif "PID_1"
[image10]: ./Gif/PID_2.gif "PID_2"
[image11]: ./Gif/PID_3.gif "PID_3"
[image12]: ./Gif/PID_4.gif "PID_4"


## Rubric points
---  

## Compilation
### The code compiles correctly.  
- Some files are changed   
PID.cpp  : It is the PID controller implementation for vehicle longitudinal / lateral control.   
main.cpp : It it the vehicle longitudinal / lateral controller using PID controller.


## Implementation
### The PID procedure follows what was taught in the lessons.  
The longitudinal / lateral motion control using a PID controller is implemented by the following method.  
- lateral controller   
1. P-error, I-error, and D-error are calculated using the CTE value, which is the lateral position error from the center of the lane.  
2. Using each error value and P-gain, I-gain, and D-gain, the final steering input value is calculated as follows.  
-> `steer_value = -p_error * Kp - i_error * Ki - d_error * Kd`

- longitudinal controller   
1. P-error, I-error, and D-error are calculated using the difference value between desired speed value in the range of 20 mph to 35 mph and the current speed.  
2. The desired speed decreases when the CTE value is more than a certain value, and increases when the lane is well maintained.
2. Using each error value and P-gain, I-gain, and D-gain, the final throttle input value is calculated as follows.  
-> `throttle_value = -p_error * Kp - i_error * Ki - d_error * Kd`

## Reflection
### Describe the effect each of the P, I, D components had in your implementation.
- P-gain  
If only P-gain is used, overshoot can occur and unstable movement is shown.  
### [P-gain, I-gain, D-gain] = [0.1, 0, 0] 

![alt text][image1]

However, if you decrease the P-gain value to reduce this movement, it will not follow the corner well as follows.   


### [P-gain, I-gain, D-gain] = [0.05, 0, 0] 

| start | curv1 | curv2 | 
|:--------:|:------:|:---------------:|
| ![alt text][image2] | ![alt text][image3] | ![alt text][image4]  | 

- D-gain  
If D-gain is added, it is possible to calculate the input by changing the error. Therefore, straight running performance and cornering performance are improved.

### [P-gain, I-gain, D-gain] = [0.155, 0, 3] 
| start | curv1 | curv2 | curv3 |
|:--------:|:------:|:---------------:|:----:|    
| ![alt text][image5] | ![alt text][image6] | ![alt text][image7]  | ![alt text][image8]|

- I-gain  
Adding I-gain improves the steady-state error. Therefore, it makes it easier to drive the center of the car when cornering.
### [P-gain, I-gain, D-gain] = [0.155, 0.0008, 3] 
| start | curv1 | curv2 | curv3 |
|:--------:|:------:|:---------------:|:----:|    
| ![alt text][image9] | ![alt text][image10] | ![alt text][image11]  | ![alt text][image12]|  


### Describe how the final hyperparameters were chosen. [P-gain, I-gain, D-gain] = [0.155, 0.0008, 3]
PID gain tuning was performed through the manual method, and tuning was performed in the following procedure.
- P-gain selection within stable range
- Additional selection of D-gain for track driving
- Additional selection of I-gain to improve the track center driving performance
- Fine tuning for each P-gain, I-gain, D-gain  




## Reflection
### The vehicle must successfully drive a lap around the track.
- clear 



-----






# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

