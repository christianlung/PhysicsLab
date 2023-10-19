# Physics Lab Final Project
The premise of this lab is to explore the effects of springs in parallel and serial on the period of an oscillating system. Through the evaluation of Hooke's law and effective spring constant equations, we predicted that parallel springs will have 1/√ 2 times the normal period and serial springs will have √ 2 times the normal period. Using data filtering, reduction, and fitting, we are able to support our original hypotheses.

## Data Cleaning
- Import the necessary libraries
```
import numpy as np
import scipy.optimize as sp
import matplotlib.pyplot as plt
```
- Standardize and filter data
```
n1_s = n1[:,0]/1000
n1_m = n1[:,1]/100
n1_time_window = n1_s[162:251] - n1_s[162]
n1_position_window = n1_m[162:251]
```
**Note that this is one of three trials conducted the normal spring configuration

- Data reduction
```
n_mean_time_window = n1_time_window[0:89]
n_mean_position_window = (n1_position_window+n2_position_window+n3_position_window)/3
```
**This step combines all three normal spring configuration trials into one average**

## Data Fitting
Scipy works by inputting the recorded time and distance as well as guess parameters for the function, so that it can have an educated guess regarding the best true parameters.
- To guess the parameters, simulate the following:
```
guess_n_mean_amplitude = (np.max(n_mean_position_window) - np.min(n_mean_position_window))/2
guess_n_mean_angular_frequency = 2*np.pi/0.7
guess_n_mean_phase = 5
guess_n_mean_offset = np.mean(n_mean_position_window)
guess_n_mean_params = [guess_n_mean_amplitude, guess_n_mean_angular_frequency, guess_n_mean_phase, guess_n_mean_offset]
```
This generates an array of the guess parameters.

- To generate the best true parameters and extract the fitted function:
```
optimal_n_mean, n_mean_covariance = sp.curve_fit(
    sin_fit_function, n_mean_time_window, n_mean_position_window, p0 = guess_n_mean_params)
fitted_n_mean = sin_fit_function(n_mean_time_window, *optimal_n_mean)
```

## Data Visualization
- To visualize the filtered and reduced data
```
plt.plot(n_mean_time_window, fitted_n_mean - np.mean(fitted_n_mean), label="Normal", color = "red")
plt.plot(p_mean_time_window, fitted_p_mean - np.mean(fitted_p_mean), label="Parallel", color = "#B8E9FB")
plt.plot(s_mean_time_window, fitted_s_mean - np.mean(fitted_s_mean), label="Serial", color = "#CEC5FA")
plt.title("Spring Oscillation (Normal, Serial, Parallel)")
plt.xlabel("Time (s)")
plt.ylabel("Position (m)")
plt.legend()
```

**The optimal fitted mean is subtracted with the original means to center data around the x axis for ease of comparison.

**One can clearly see that the image demonstrates parallel and serial spring configurations and their effect on the period.**
![Graphs of fitted curves](https://github.com/christianlung/PhysicsLab/blob/main/Configuration%20Graph.png?raw=true)
