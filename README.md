# Sensors And Control Assessment4

**Members:** Benjamin Li 26111296 | Aidan Lee 25918525   
**Video:** https://youtu.be/La9iTg0fb1c

## Contributions
Aidan
* **Model (A):** state `[x, y, theta]`, input `(v, w)`. Error model `e_y' = v e_theta`, `e_theta' = w - v kappa`. Controllability and observability ranks are in `results.txt`.
* **Sensing (B):** calibrated by standing still at known ranges: range sigma 1.5 cm at 1 m to 11 cm at 5 m, bearing 0.01 rad. Frame change: the camera is offset 5 cm from the robot centre, handled in `camera_position`.

Benjamin
* **Estimation (C):** EKF, odometry from wheel-encoder increments (0.02 m/s, 0.05 rad/s assumed) plus a small heading drift term for the worn left wheel the encoders cannot see.
* **Control (D):** LQI on `[e_y, e_theta, z]`, Bryson limits 10 cm, 10 deg, 1 rad/s, 0.15 m s; feedforward `v kappa`; integrator frozen when clipped or avoiding.
* **LiDAR:** if anything is within 0.35 m ahead the robot slows and steers to the freer side; the planner's map does not contain the `surprise` obstacle.

## Generative AI declaration
Google Gemini was used to generalise and explain the thought process behind the production on the assesment in general. It did not write a line of code.
