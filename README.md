# 针对四旋翼位置控制的双环PID 位置控制器 

# Two-loop PID controller for quadtotor UAV position control

Base on MATLAB SIMULINK.

Fireup the UAV-DONE.slx file to review the controller and evaluate its performance.

All dynamic transformation function regarding to the UAV are embedded.



## Definition of the Coordination  

The coordinate of the UAV is defined as follows, in which coordinate E(OXYZ) is fixed to the ground and the origin of B(oxyz) is coincident to the origin of the UAV. It is assummed that the structure of the UAV is symmetric.

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/coordinate%201.png)

 The angle $ \phi , \theta, \psi $ are defined as the roll angle, the pitch angle and the yaw angle of the UAV, respectively.

The definition of these angle and their relation with the coordinate is shown as follows: 

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/coordinate%202.png)

## UAV and rotor paramters

The basic charateristic of the UAV can be listed as follows:

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/UAV%20characteristic%20parameters.png)



, where $ m \ [KG], l \ [meter] $ are the weight of the UAV and the  distance between the origin of the UAV and the rotation center of the roter, respectively.

$ I_x, I_y, I_z $are the rotation inertia of the UAV around the correspondent axis of the  subscript, espectively.

The  propeller in the UAV is  *Draganflyer III* from  *EPFL OSA I* .

The rotor has  is leanerised in the range of $ \pm 2600 \ rpm$ and modified as an  inertia link with the time constant $T_s$ of 0.005s (the time for the rotor to increase $ 1\ rpm$).  

For other specific parameters please refer to the SIMULINK model for more details. 

## Control Objective

PID parameters are tuned to fit the following control requirement:

- when tracing the unit step position input command, the output overshoot $ \sigma <=20 \% $ , the settling time $ t_s <=0.5s$
- when tracing the unit step angular input command, the output overshoot $ \sigma <=20 \%$ , the settling time $ t_s <=0.5s$



## Dynamic model of the UAV

Here we omit the derivation of the dynamic model of the UAV for comciseness and purpose the dynamic model in SIMULINK derictly as follows:

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/Simulation%20model.png)

The control commends from the controller will first go through the $U-w$ model and be converted to the control voltage of the  rotor.

Secondly, the rotor is simulated with a inertia link  and a saturation link to maintain response linearity.

Then the corresopnding thrust from the rotors will be converted to the acceleration of the pith angle $\theta$,  the acceleration of the roll angle $\phi$,  the acceleration of the yaw angle $\psi$ and the total pulling force, respectively.   

These accelerations will be further integrated to get the pith angle $\theta$,  the roll angle  and  yaw angle $\psi$ and further sent to the dynamic model of the UAV, in which they are considered with the pulling force to get the acceleration in $X \ Y \ Z $ axis. 

Finally the acceleration on these axis will be integrated to get the corresponding disposition $x \ y \ z$ .

## PID controller construction

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/PID%20control%20structure.png)

Based on the dynamic charateristic of the quadrotor UAV, its linear displacement is a result of its angular displacement. The UAV must firstly lean on a particular angle to enable the  linear displacement, which is the result of the angular leaning and the corresponding unbalanced pull.

Thus, a two-loop controller is designed to successfully control the position displacement by firstly controling the angle with the inner loop to yield the proper angular displacement for the outter loop to control the position.



In the graph of the controller above, the subscript $_d$ denotes the desired valued (control value) that the UAV need to track with its motion. $U_i \ i=1,2,3,4$ stand for the controlling voltage of the rotor $i$ , respectively.  

## Experimental Evaluation of the controller 

### position control experiment

In this experiment, the UAV needs to track particular position controll command given by $X_d \ Y_d$ and $Z_d$.

For example, in the experiment with the subscript $x$, $X_d$ is a unit step controll input while $Y_d$ and $Z_d$ remain $0$. And the same is for expriment $y$ and $z$.

The result is shown as follows:

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/position%20control%20exp.png)

As we can observe from the experiments, when the UAV needs to track indepent controll command on the axis, the maximum overshoot $\sigma$ and the settling time $t_s$ satisfies the controll objective, which indicates the success of the controller design.

### Dynamic tracking experiment

In this experiment, the UAV needs to trace a dynamic control input, which is a spatial helix, to demonstrate its ability to trace dynamic contol command.

The helix follows the equation of : $x_d= sin(\pi t), \ y_d=cos(\pi t)$ and $z_d=0.5t$ which is helix with diameter $d=2m$ and picth =   $0.5m/s$.

The result is shown as follows:

![](https://github.com/sadwfi/PID-controller-for-quadtoter-UAV/raw/master/dynamic%20tracking%20exp.png)

From the expriment result we can see that, even under challenging control requirement, the designed controller can still successfully control the UAV to track the dynamic control commend.