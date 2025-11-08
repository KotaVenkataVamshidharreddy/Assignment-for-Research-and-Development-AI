# Assignment-for-Research-and-Development-AI
flam-assignment
# Parametric Curve Fitting Optimization

This repository contains the solution and methodology for finding the unknown parameters of a complex parametric curve based on a given set of `(x, y)` data points.

## 1. Problem Statement

The goal was to find the unknown variables $\theta$, $M$, and $X$ for the following parametric equations:

$$
x = \left(t \cdot \cos(\theta) - e^{M\left|t\right|} \cdot \sin(0.3t) \cdot \sin(\theta) + X \right)
$$

$$
y = \left(42 + t \cdot \sin(\theta) + e^{M\left|t\right|} \cdot \sin(0.3t) \cdot \cos(\theta)\right)
$$

The search was constrained by the following bounds:
* $0^{\circ} < \theta < 50^{\circ}$
* $-0.05 < M < 0.05$
* $0 < X < 100$

The parameter $t$ ranges from 6 to 60, and a set of `(x, y)` points that lie on this curve was provided in the `xy_data.csv` file.

## 2. Methodology

The core challenge is that for each point $(x_i, y_i)$ in the data, the corresponding value of $t_i$ is unknown. To solve this, a critical assumption was made.

### 2.1. The "Ordered $t$" Assumption

We assumed that the $N$ points in `xy_data.csv` are sorted and correspond to $N$ linearly spaced values of $t$ in its given range. Therefore, we generated our $t$ vector as:

`t_values = np.linspace(6.0, 60.0, num_points)`

This maps the first data point to $t=6$, the last data point to $t=60$, and all other points in between.

### 2.2. Objective Function (L1 Loss)

As per the assignment's assessment criteria, the objective function was defined as the total **L1 (Manhattan) distance** between the predicted curve and the given data. This is the sum of the absolute errors in both $x$ and $y$:

$$
\text{Error} = \sum_{i=1}^{N} \left| x_{\text{data},i} - x_{\text{model},i} \right| + \left| y_{\text{data},i} - y_{\text{model},i} \right|
$$

Our goal was to find the parameters $(\theta, M, X)$ that minimize this error.

### 2.3. Optimization Strategy

To ensure a robust and globally optimal solution was found, five different optimization algorithms from the `scipy.optimize` library were employed:

1.  **L-BFGS-B:** A fast, local "hill-climbing" optimizer.
2.  **SLSQP:** Another fast, local optimizer that is good with constraints.
3.  **Basin-Hopping:** A global optimizer that "hops" between different local minima to find a better one.
4.  **SHGO (Simplicial Homology Global Optimization):** A global optimizer designed for complex, non-linear problems.
5.  **Differential Evolution:** A population-based global optimizer that is very robust.

Running all five and comparing their results gives us high confidence. If they all converge to the same answer (as they did here), we can be certain it is the true global minimum.

## 3. Results

All five optimization methods converged to virtually the same set of parameters, with the L1 error being almost identical. This is a strong indication that we have found the true global minimum.

The best-performing algorithm was **SHGO**, which gave the following results:

* **Best L1 Error:** $37,865.09$
* **Parameters:**
    * $\theta \text{ (degrees)} = 28.118423^{\circ}$
    * $M = 0.021389$
    * $X = 54.900299$

### 3.1. Visual Verification

The plots generated confirm the high quality of the fit. The `best_fit_vs_data.png` image shows the predicted curve (in red) lying almost perfectly on top of the original data points (blue), capturing the distinct spiral/oscillating nature of the curve.

![Best Fit Curve vs. Original Data](best_fit_vs_data.png)

The comparison plot, `all_approaches_vs_data.png`, further confirms that all five methods produced visually indistinguishable curves.

### 3.2. Final Equations

For submission, the best-fit $\theta$ value must be converted to radians:
$\theta \text{ (radians)} = 28.118423^{\circ} \cdot \frac{\pi}{180^{\circ}} \approx 0.490759$

Standard Format:

$$
x = t \cos(0.490759)
  - e^{0.021389|t|}
    \sin(0.3t)
    \sin(0.490759)
  + 54.900299
$$

$$
y = 42
  + t \sin(0.490759)
  + e^{0.021389|t|}
    \sin(0.3t)
    \cos(0.490759)
$$




**Desmos Format:**
x = t * cos(0.490759) - e^(0.021389*abs(t)) * sin(0.3*t) * sin(0.490759) + 54.900299 


y = 42 + t * sin(0.490759) + e^(0.021389*abs(t)) * sin(0.3*t) * cos(0.490759)

