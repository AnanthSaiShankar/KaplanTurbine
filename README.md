
# Kaplan Turbine 

This project is aimed to design a Kaplan Turbine using MATLAB and Solidworks.


## Introduction
Kaplan Turbine works on the principle of axial flow reaction. Water travels through the runner in axial flow turbines in a direction parallel to the runner's axis of rotation.
The Kaplan Turbine is now widely employed for high flow and low head power production all over the world. The turbine shaft is vertical in an axial flow reaction turbine. The hub, which is located at the bottom end of the shaft, is made larger. Because the vanes are fastened to the hub, the hub serves as the axial flow reaction turbine's runner.

## MATLAB
A MATLAB code is used to generate the blade profiles of the Kaplan Turbine.
Runner Blade crossection is to be selected, Here we have used NACA 4412 
Input Parameters like Power Output, Available   Head, Total Efficiency, Number of blades and Optimum angle of attack must be given.

```bash
%Inputs
P = input('Power Output \n'); % Power Output in KW
H = input('Available Head \n'); % Available Head in m
eff = input('Total Efficiency \n'); % Total Efficiency
z = input('Number of Blades \n'); % Number of blades
attack = input('Optimum Angle of Attack \n'); % Optimum Angle of Attack in deg

density = 1000;
bladesections = 5;

Q = P.*1000./(eff.*density.*9.81.*H); % Discharge
Ns = 885.5/(H.^0.25); % Specific Speed
N = Ns.*(H.^1.25)./(P.^0.5); % Turbine Speed
phi = 0.0242.*(Ns.^(2./3));
d_run = 84.5.*phi.*(H.^0.5)./(N); % Runner Diameter
m = 0.4;
d_hub = m.*d_run; % Hub Diameter

flowarea = pi.*((d_run.^2)-(d_hub.^2))./4;
V_f = Q./flowarea; % Flow Velocity
d_avg = (d_run + d_hub)./2;
V_avg = pi.*d_avg.*N./60; % Blade Velocity
V_w = P.*1000./(density.*Q.*V_avg); % Whirl Velocity

s = linspace(1.3,0.75,bladesections);
i = 1;
for d = linspace(d_hub,d_run,bladesections)
    U = (pi.*d.*N)./60;
    beta_1 = atand(V_f./(U-V_w)); % Inlet Blade Angle
    beta_2 = atand(V_f./U); % Outlet Blade Angle
    t = (pi.*d)./z; % Blade Spacing
    chord = s(i).*t;
    theta = 180 - beta_1 + attack; % Angle of Rotation
    [naca4412] = xlsread('NACA4412_Dat_File.xlsx');
    x = naca4412(:,1);
    y = naca4412(:,2);
    x = x*chord;
    y = y*chord;
    x = x - (chord/2);
    R = [cosd(theta) sind(theta); -sind(theta) cosd(theta)];
    rot_mat = R*[x';y'];
    X_cord = rot_mat(1,:)';
    Y_cord = rot_mat(2,:)';
    Z_cord = zeros(35,1);
    X_cord = round(X_cord,6);
    Y_cord = round(Y_cord,6);
    figure
    plot(X_cord,Y_cord);
    set(gcf,'position',[0,0,800,800])
    section = [X_cord Y_cord Z_cord];
    writematrix(section,['section' num2str(i) '.txt'], 'delimiter', 'tab')
    i = i+1;
end
```
Blade profiles are generated for different sections. Blade profile at each section have different values of chord length.
The output of this MATLAB code is given as 5 text files containing coordinates of different sections of Blade Profile.   

## SOLIDWORKS
This coordinates generated by MATLAB is imported into SOLIDWORKS. Blade profiles are created at different section using Curves through x, y & z points command. Thus the runner blades and hub is designed using Lofted Boss/Base and Extruded Boss/Base commands in SOLIDWORKS.  
    
## Software Versions
- MATLAB R2021a
- SOLIDWORKS 2021 SP0.0