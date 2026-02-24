# Data Generation using PyBullet Physics for Machine Learning

##  Project Overview
This project demonstrates the use of **PyBullet** for synthetic data generation to solve a physical regression problem. We simulate a rigid-body "drop test" to create a dataset of falling objects, then train several Machine Learning models to predict the **initial drop height** based on impact metrics. To make the task realistic, the simulation includes environmental variance and simulated sensor noise.

##  Methodology

### 1. Simulation Setup
* **Tool:** PyBullet Physics Engine.
* **Environment:** A 3D world with gravity set to $-9.81 m/s^2$.
* **Task:** 1,000 simulations of a cube dropped from varying heights.

### 2. Parameter Bounds & Data Generation
We defined specific ranges to ensure diverse data while maintaining physical constraints:
* **Drop Height (Target):** $0.5$m to $10.0$m.
* **Orientation:** Randomized Euler angles ($0$ to $2\pi$ rad).
* **Sensor Realism:** 2% Gaussian noise was added to the recorded `Time to Impact` and `Final Velocity` to simulate real-world measurement error.
* **Environmental Variance:** Minor lateral forces were applied to simulate air resistance/wind.

### 3. Machine Learning Pipeline
The generated dataset was split (80% training, 20% testing) to evaluate five different regression models:
* Linear Regression
* Random Forest Regressor
* Decision Tree Regressor
* Support Vector Regression (SVR)
* K-Nearest Neighbors (KNN)

## Results & Model Comparison

Based on the simulation run, the models performed as follows:

| Model | MAE (Mean Absolute Error) | R² Score |
| :--- | :--- | :--- |
| **Random Forest** | **0.3134** | **0.9463** |
| **Decision Tree** | 0.3667 | 0.9253 |
| **SVR** | 0.3761 | 0.8838 |
| **KNN** | 0.4367 | 0.9019 |
| **Linear Regression** | 0.6307 | 0.8848 |

## 📈 Analysis
* **Sim-to-Real Gap:** The $R^2$ score of ~0.94 is highly realistic. It shows the models can handle the 2% measurement noise and environmental drag successfully.
* **Optimal Model:** **Random Forest** provided the best performance, as it is naturally suited for capturing the non-linear relationship ($d = \frac{1}{2}gt^2$) present in falling body physics.
