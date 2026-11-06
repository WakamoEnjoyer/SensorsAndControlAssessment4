# Sensors And Control Assessment4

**Members:** Benjamin Li 26111296 | Aidan Lee 25918525   
**Video:** https://youtu.be/La9iTg0fb1c

## How to run

The notebook is built for **Google Colab** (Linux). It will not run on Windows locally.

How to test it in a fresh Colab
1. Download both files above (TrackA_WafflePi_RunAll, TrackA_Tutor_Ready).
2. Open the .ipynb notebook in Google Colab.
3. Select Runtime → Run all.
4. When prompted, upload TrackA_Tutor_Ready.zip.
5. Wait for the navigation results, trajectory graph and 3D video.

## How it works

| Part | What we did |
|---|---|
| **Model** | State `[x, y, theta]`, input `(v, w)` (unicycle). Path-error model `e_y' = v e_theta`, `e_theta' = w - v kappa`. |
| **Sensing** | Camera measures range and bearing to ArUco markers. Noise was calibrated from known distances. The camera sits 5 cm from the robot centre, handled as a frame change in the measurement model. |
| **Estimation** | EKF: predict from encoder odometry, update from each marker detection. Innovation gating rejects outliers. Consistency checked with NEES/NIS. |
| **Control** | State-feedback (LQR/LQI) on the path error, with curvature feedforward, using the EKF estimate. Gains chosen with Bryson's rule. |
| **Obstacles** | A* path planning with spline smoothing around the known obstacles. |

## Repository layout

```
TrackSimulation.ipynb   notebook that runs everything
tb3_trackA_v2.zip       code package (tb3nav/) the notebook uploads
results/                plots, data, replay video
```

`tb3nav/` modules: `ekf`, `models`, `control`, `planner`, `sim`, `vision`, `mujoco_adapter`, `metrics`, `plots`.

## Contributions
Aidan
* **Model (A):** state `[x, y, theta]`, input `(v, w)`. Error model `e_y' = v e_theta`, `e_theta' = w - v kappa`. Controllability and observability ranks are in `results.txt`.
* **Sensing (B):** calibrated by standing still at known ranges: range sigma 1.5 cm at 1 m to 11 cm at 5 m, bearing 0.01 rad. Frame change: the camera is offset 5 cm from the robot centre, handled in `camera_position`.

Benjamin
* **Estimation (C):** EKF, odometry from wheel-encoder increments (0.02 m/s, 0.05 rad/s assumed) plus a small heading drift term for the worn left wheel the encoders cannot see.
* **Control (D):** LQI on `[e_y, e_theta, z]`, Bryson limits 10 cm, 10 deg, 1 rad/s, 0.15 m s; feedforward `v kappa`; integrator frozen when clipped or avoiding.
* **LiDAR:** if anything is within 0.35 m ahead the robot slows and steers to the freer side; the planner's map does not contain the `surprise` obstacle.

## Generative AI declaration
Google Gemini was used to generalise and explain the thought process behind the production on the assesment in general. It did not write a line of code. However ChatGPT was used to optimise, debug, compile and provide suggestions for the jupyter notebook. Most notably, it was used to compile all the work into a single easy to run setup.
