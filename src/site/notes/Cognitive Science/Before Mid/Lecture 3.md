---
{"dg-publish":true,"permalink":"/cognitive-science/before-mid/lecture-3/"}
---

This lecture shifts from abstract learning concepts into applied robotics, specifically how a robot figures out where it is in the world.

# Outline
[[Cognitive Science/Before Mid/Lecture 3#**Part 1 The Problem of Uncertainty**\|The Problem of Uncertainty]]
[[Cognitive Science/Before Mid/Lecture 3#**Part 2 Odometric Error Model**\|Odometric Error Model]]
# **Part 1: The Problem of Uncertainty**

The core theme of the lecture is summarized by a quote from G.K. Chesterton: =="There is only one thing certain and that is that nothing is certain"==. A robot can never know its exact location because it constantly deals with two types of noise:

- **Sensor Noise:** Sensors are imperfect. For example, 
	- ultrasonic sensors suffer from acoustically reflective environments 
	- and multipath interference 
	- while color cameras can be confused by changes in illumination . 

- Furthermore, there is "**Sensor Aliasing**," meaning a single sensor reading is usually not unique enough to identify the robot's location on its own .

- **Effector Noise:** The robot's physical movement is also uncertain . Errors accumulate because the floor might be sloped, the wheels might slip, a human might push the robot, or the wheels might have slightly unequal diameters .

# **Part 2: Odometric Error Model**

When a robot moves, it uses "odometry" to estimate its position using internal sensors (like measuring how many times its wheels have turned). The position is tracked using $x$ and $y$ coordinates, along with its orientation angle $\theta$.

However, because of the effector noise mentioned above, small errors in the rotation of the right wheel ($\Delta S_r$) and left wheel ($\Delta S_l$) cause the robot's calculated position to drift from its actual position over time. The mathematical error model calculates this drift by factoring in the distance traveled ($\Delta s$) and the angular error ($\theta_{error}$) .
