# AMA_Solo_SM
Additional materials for project on Newton-Raphson in N dimensions


Contents
1. Derivation for generalising Newton-Raphson in N dimensions photo
2. Plots for finding the roots for Eq(4) using different initial $x0$ values photo
3. Error plot for Eq(4) showing the gradient of $2$ corresponding to the order of convergence
4. Python code used for Newton-Raphson method
5. MatLab code used for 3D plot showing order of convergence



-------------------------....--------------------------...---------------------------...-------------


5. Matlab code
function newton2D()

    clc
    close all

    x = -10:0.1:10;
    y = -10:0.1:10;

    [xx, yy] = meshgrid(x, y);
    zz = f(xx, yy);

    figure;
    mesh(xx, yy, zz);
    xlabel('X-axis');
    ylabel('Y-axis');
    zlabel('Z-axis');
    hold on

    %%% Finding a root where f(x, y) = 0

    r0 = [-10; 10];
    alpha = 0.01; % Adjusted alpha
    tolerance = 1e-5;

    iteration_count = 0;
    errors = [];

    while abs(f(r0(1), r0(2))) > tolerance
        iteration_count = iteration_count + 1;
        errors(iteration_count) = abs(f(r0(1), r0(2)));

        r0 = r0 - alpha * f(r0(1), r0(2)) ./ fprime(r0(1), r0(2));
    end

    disp('Root where f(x, y) = 0:');
    disp(r0);
    disp('Value of f at the root:');
    disp(f(r0(1), r0(2)));
    disp(['Number of iterations: ', num2str(iteration_count)]);

    % Calculate the order of convergence
order_of_convergence = zeros(1, iteration_count - 2);

for i = 1:iteration_count - 2
    order_of_convergence(i) = log(errors(i) / errors(i + 1)) / log(errors(i + 1) / errors(i + 2));
end

% Check if there are enough elements for the order calculation
if iteration_count > 2
    disp(['Estimated order of convergence: ', num2str(mean(order_of_convergence))]);
else
    disp('Insufficient iterations to calculate the order of convergence.');
end
    plot3(r0(1), r0(2), 0, 'rs', 'MarkerSize', 20);

    % Plotting a circle
    theta = linspace(0, 2 * pi, 100);
    r = 5;
    xc = r * cos(theta);
    yc = r * sin(theta);

    plot3(xc, yc, 0 * xc, 'r', 'LineWidth', 2)

    legend('f(x,y)', 'Root', 'Circle', 'Location', 'Best');

    function z = f(x, y)
        z = x.^2 + y.^2 - 5^2;
    end

    function zprime = fprime(x, y)
        % Gradient which is a vector
        dfdx = 2 * x;
        dfdy = 2 * y;

        zprime = [dfdx; dfdy];
    end

    function zdblprime = fdblprime(x, y)
        % Hessian which is a matrix
        df2dx2 = 2;
        df2dxdy = 0;
        df2dy2 = 2;
        df2dydx = 0;
        zdblprime = [df2dx2, df2dxdy; df2dydx, df2dy2];
    end
end
