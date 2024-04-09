# Freefall-Simulator
Using MATLAB to perform calculation for the velocity and acceleration of a falling object. 

# CODE

~~~ matlab
function [time, termVelocity] = freefall(mass, height, drag)
    % FREEFALL - Calculate fall time, terminal velocity, and plot position,
    % velocity, acceleration using mass, height, and drag inputs
    %
    % INPUT
    % mass - mass of the object being dropped (kg)
    % height - height at which the object is being dropped (m)
    % drag - coefficient of drag of the object being dropped (kg/s)
    %
    % OUTPUT
    % time - time it takes for object to reach the ground
    % velocity - terminal velovity of object

    % PART 1: Inputs
    % Input 1: mass (kg)
    % Input 2: height (m)
    % Input 3: coefficient of drag (drag) (kg/s)

    % Minimum nargin check
    if nargin ~= 3
        error("ITP168:nargin", "3 Inputs are required (mass, height, drag coef)")
    end
    % Validate input 1,2,3
    for n = 1:3
        if n == 1
            input = mass;
        elseif n == 2
            input = height;
        else
            input = drag;
        end

        if input<=0 || ~isnumeric(input) || ~isscalar(input) 
            error("ITP168:input", "Input needs to be greater than zero, numeric, and scalar value")
        end
    end

    % PART 2: Calculate terminal veocity
    % Set F(g) = F(air)
    % mg = Cd*V
    % Find V
    termVelocity = (mass*9.81)/drag;

    % PART 3: Determine fall time
    % increasing the time interval (+0.01s) would increase accuracy of time, but that would require more processing power
    % t = increasing time vector [end, t(end)+0.01]
    % v = velovity at any point
    distance = 0;
    n = 2;
    t = [0,0.01];
    while distance < height
        v = ((mass*9.81)/drag)*(1-exp((-drag*t)/mass));
        distance = trapz(t,v);
        if distance <= height 
        t(n+1) = t(n) + 0.01;
        n = n+1;
        end
    end
    time = t(end);

    % PART 4: Plot the fall
    % Find acceleration of each point in time
    % Differentiate velocity for acceleration
    accelX = diff(t);
    accelY = diff(v);
    acceleration(1) = 9.81;
    acceleration(2:length(v)) = accelY./accelX;

    % Find altitude
    position = height - cumtrapz(t,v);

    % Open figure window 
    figure()
    
    % Plot graphs
    % Time(s) vs Height(m)
    subplot(3,1,1)
    plot(t,position)
    title(sprintf('Freefall With Air Resistance: C_D = %.3f, V_t = %.2f m/s',drag, termVelocity));
    ylabel('Height (m)');
    xlabel('Time (s)');

    % Time(s) vs Velocity(m/s)
    subplot(3,1,2)
    plot(t,v)
    ylabel('Velocity (m/s)');
    xlabel('Time (s)');

    % Time(s) vs Acceleration(m/s^2)
    subplot(3,1,3)
    plot(t,acceleration)
    ylabel('Acceleration (m/s^2)');
    xlabel('Time (s)');
end
~~~

# DEMO
![image](https://github.com/lenduong/Freefall-Simulator/assets/141017307/9defdfd5-74dd-4758-9d44-6e90428d3844)
