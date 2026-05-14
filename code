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

