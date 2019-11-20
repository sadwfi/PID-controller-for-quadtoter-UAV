# 针对四旋翼位置控制的双环PID 位置控制器 

# Two-loop PID controller for quadtotor UAV position control

Base on MATLAB SIMULINK.

Fireup the UAV-DONE.slx file to review the controller and evaluate its performance.

All dynamic transformation function regarding to the UAV are embedded.



The coordinate of the UAV is defined as follows: 

E(OXYZ) is fixed to the ground and the origin of B(oxyz) is coincident to the origin of the UAV

 



The basic charateristic of the UAV can be listed as follows



PID parameters are tuned to fit the following control requirement:

- when tracing the unit step position input command, the output overshoot $ \sigma $ <=20%, the settling time ts <=0.5s
- when tracing the unit step angular input command, the output overshoot \sigma <=20%, the settling time ts <=0.5s

