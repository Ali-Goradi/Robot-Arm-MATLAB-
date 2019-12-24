# Robot-Arm-MATLAB-
clc
close all
clear all
load matlab1.mat
%% Arduino encoder for 6 servo motors 
a = arduino(); %% or use this a = arduino('COM3', 'Uno', 'Libraries', 'Servo');
ServoBase   = servo(a, 'D11', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)
ServoSecond = servo(a, 'D10', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)
ServoThird  = servo(a, 'D9', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)
ServoFourth = servo(a, 'D6', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)
ServoFifth  = servo(a, 'D5', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)
ServoSixth  = servo(a, 'D3', 'MinPulseDuration', 700*10^-6, 'MaxPulseDuration', 2300*10^-6)

Z0=87;
pi=3.14159265359;
X=[5 9 9 12 12 12 15 15 15 15];
Y=[4 1 1 5 5 5 -3 -3 -3 -3];
Z=[0 0 0 0 -10 -10 -10 20 12 12];
Q=[150 150 50 50 50 50 50 50 150 150];
C=[0 0 0 0 0 180 90 90 90 0];
for i = [1 2 3 4 5 6 7 8 9 10]
Theta_base=87+atan(Z(1,i)/X(1,i))*(180/pi);
[T0 T1]=sim(net1,[X(1,i);Y(1,i)]);  %NN Inpus and outputs
p=T0(1,1);
q=T0(2,1);
%%[0-180]-mapping-->>>[0,1]
    writePosition(ServoBase, abs((Theta_base)/180));    %fixed at z=0;
    writePosition(ServoSecond, 0/180);   %fixed
    writePosition(ServoThird, abs(p)/(180));   %output neural 1
    writePosition(ServoFourth,abs(q)/(180));  %output neural 2
    writePosition(ServoFifth, Q(1,i)/180);  % perpendecular at 50, while the ref angle is 150
    writePosition(ServoSixth, C(1,i)/180);   % 90=catch, 0=open
    pause(2); %% delay time of seconds
   i
end  
