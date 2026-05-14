# Drone-Altitude-Stabilization
Drone Altitude Stabilization is a control systems project focused on maintaining a drone at a desired height automatically despite external disturbances such as wind or sudden force changes. In this system, the drone’s vertical motion is  represents the relationship between thrust input and altitude output.
This project focuses on designing and simulating a Drone Altitude Stabilization System using MATLAB and Control System Toolbox. The objective is to maintain a drone at a desired altitude automatically, even in the presence of external disturbances such as wind. A PID (Proportional-Integral-Derivative) controller is implemented to improve system stability, reduce overshoot, minimize settling time, and eliminate steady-state error.
The drone’s vertical dynamics are modeled as a second-order transfer function, and both open-loop and closed-loop responses are analyzed. Wind disturbance is introduced during simulation to evaluate the robustness of the controller.

Problem Statement
Design a control system for a drone that:
-Maintains desired altitude accurately
-Overshoot < 10%
-Settling Time < 3 seconds
-Steady-State Error ≈ 0
-Remains stable under external disturbances

System Model
The drone altitude system is represented by:
G(s)=1/s2+2s+5
Inputs:Thrust Command
Output:Drone Altitude
Disturbance:Wind force introduced at 5 seconds

Features
Open-loop system analysis
PID controller design and tuning
Closed-loop step response analysis
Wind disturbance simulation
Performance metrics evaluation
Root locus and Bode plot analysis

Tools & Technologies
MATLAB
Control System Toolbox
Transfer Function Modeling
PID Controller

Project Workflow
1.Define drone transfer function
2.Analyze open-loop response
3.Design PID controller
4.Create closed-loop feedback system
5.Simulate altitude response
6.Add wind disturbance
7.Evaluate performance

Performance Metrics
The system is evaluated using:
-Rise Time
-Settling Time
-Overshoot
-Steady-State Error
-Disturbance Rejection

Expected Results
After PID implementation:
-Faster altitude response
-Reduced oscillations
-Stable hover performance
-Quick recovery from wind disturbance

Sample Output Graphs
-Open Loop Step Response
-Closed Loop Step Response
-Wind Disturbance Response
-Root Locus
-Bode Plot

How to Run
1.Open MATLAB
2.Copy the provided .m code into a new script
3.Run the script
4.Observe generated graphs and performance data

Applications
-Autonomous drones
-Aerospace control systems
-Robotics
-UAV flight stabilization
-Research in control engineering

Future Improvements
-Adaptive PID tuning
-Fuzzy Logic Controller
-LQR Control
-Real-time sensor integration
-Simulink 3D visualization

Conclusion
This project demonstrates how PID control can effectively stabilize drone altitude in dynamic environments. By using feedback control, the drone can maintain a fixed height, reject disturbances, and improve safety and reliability. It is an excellent beginner-to-intermediate project for hackathons, academic demonstrations, and practical control systems learning.

code:-
clc;
clear;
close all;

%% Define Transfer Function
s = tf('s');
G = 1/(s^2 + 2*s + 5);

disp('Open Loop Transfer Function:');
G

%% Open Loop Step Response
figure;
step(G);
grid on;
title('Open Loop Step Response of Drone Altitude System');

%% PID Controller Design
% You can manually tune these values if needed
Kp = 18;
Ki = 10;
Kd = 4;

C = pid(Kp, Ki, Kd);

disp('PID Controller Parameters:');
C

%% Closed Loop System
T = feedback(C*G,1);

%% Closed Loop Step Response
figure;
step(T);
grid on;
title('Closed Loop Step Response with PID Controller');

%% Performance Analysis
info = stepinfo(T);

disp('Closed Loop Performance Metrics:');
disp(info);

fprintf('Overshoot: %.2f %%\n', info.Overshoot);
fprintf('Settling Time: %.2f sec\n', info.SettlingTime);
fprintf('Rise Time: %.2f sec\n', info.RiseTime);

%% Disturbance Analysis (Wind Disturbance at t = 5 sec)
t = 0:0.01:15;

% Step input for desired altitude
u = ones(size(t));

% Disturbance signal (wind force introduced at 5 sec)
disturbance = zeros(size(t));
disturbance(t >= 5) = 0.5;

% System response without disturbance
[y1, t1] = lsim(T, u, t);

% Disturbance transfer function
Td = feedback(G, C);

% Disturbance response
[y2, t2] = lsim(Td, disturbance, t);

% Total response
y_total = y1 + y2;

%% Plot Disturbance Response
figure;
plot(t, y_total, 'LineWidth', 2);
hold on;
plot(t, u, '--');
grid on;
xlabel('Time (seconds)');
ylabel('Altitude');
title('Drone Altitude Response with Wind Disturbance');
legend('Actual Altitude','Desired Altitude');

%% Root Locus (Optional for Analysis)
figure;
rlocus(G);
grid on;
title('Root Locus of Drone System');

%% Bode Plot (Optional for Stability)
figure;
bode(T);
grid on;
title('Bode Plot of Closed Loop System');

disp('Project Simulation Complete!');
