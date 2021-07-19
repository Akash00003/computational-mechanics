---
jupytext:
  formats: notebooks//ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.12
    jupytext_version: 1.6.0
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')
```

# Computational Mechanics Project #01 - Heat Transfer in Forensic Science

We can use our current skillset for a macabre application. We can predict the time of death based upon the current temperature and change in temperature of a corpse. 

Forensic scientists use Newton's law of cooling to determine the time elapsed since the loss of life, 

$\frac{dT}{dt} = -K(T-T_a)$,

where $T$ is the current temperature, $T_a$ is the ambient temperature, $t$ is the elapsed time in hours, and $K$ is an empirical constant. 

Suppose the temperature of the corpse is 85$^o$F at 11:00 am. Then, 2 hours later the temperature is 74$^{o}$F. 

Assume ambient temperature is a constant 65$^{o}$F.

1. Use Python to calculate $K$ using a finite difference approximation, $\frac{dT}{dt} \approx \frac{T(t+\Delta t)-T(t)}{\Delta t}$.

```{code-cell} ipython3
dTdt = (74-85)/2
constant_K = (-1) * dTdt * (1/(74-65))
print(constant_K)
```

2. Change your work from problem 1 to create a function that accepts the temperature at two times, ambient temperature, and the time elapsed to return $K$.

```{code-cell} ipython3
def find_K(temp1, temp2, tempAmb, time_elapsed):
    dTdt = (temp2-temp1)/time_elapsed
    constant_K = (-1) * dTdt * (1/(temp2-tempAmb))
    return constant_K
```

```{code-cell} ipython3
find_K(85,74,65,2)
```

3. A first-order thermal system has the following analytical solution, 

    $T(t) =T_a+(T(0)-T_a)e^{-Kt}$

    where $T(0)$ is the temperature of the corpse at t=0 hours i.e. at the time of discovery and $T_a$ is a constant ambient temperature. 

    a. Show that an Euler integration converges to the analytical solution as the time step is decreased. Use the constant $K$ derived above and the initial temperature, T(0) = 85$^o$F. 

    b. What is the final temperature as t$\rightarrow\infty$?
    
    c. At what time was the corpse 98.6$^{o}$F? i.e. what was the time of death?

```{code-cell} ipython3
import numpy as np
import math
import matplotlib.pyplot as plt
total_time = 4 #hours
def tempValues(steps):
    t_euler = []
    t_time = []
    
    t_euler.append(85)
    oldTemp = 85
    t_time.append(0)
    oldTime = 0
    
    interval = total_time/steps
    for i in range(steps):
        oldTime += interval
        oldTemp = 5*((7942*interval) - 200*oldTemp)/(611*interval - 1000)
        t_time.append(oldTime)
        t_euler.append(oldTemp)
    t_analyitical = []
    error_vals = []
    index = 0
    for timeVal in t_time:
        newTemp = 65+20*math.exp(-0.611*timeVal)
        t_analyitical.append(newTemp)
        error_vals.append((t_euler[index] - newTemp)/newTemp)
        index +=1
    return np.mean(error_vals)



#Part A
n = []
error = []

for i in range(2,15):
  steps = int(math.pow(2,i))
  n.append(steps)
  error.append(tempValues(steps))


plt.loglog(n, error,'o')
plt.xlabel('number of timesteps N')
plt.ylabel('relative error')
plt.title('Part A')
plt.show

print("B. It would approach the ambient temp of 65 deg F")

#Part C
# 98.6 = 65 + (85-65)*e^(-0.611t)
# t ~ -0.85 
# time of death was 0.85 hr = 51.6 mins before time of discovery
```

```{code-cell} ipython3
4. Now that we have a working numerical model, we can look at the results if the
ambient temperature is not constant i.e. T_a=f(t). We can use the weather to improve our estimate for time of death. Consider the following Temperature for the day in question. 

    |time| Temp ($^o$F)|
    |---|---|
    |6am|50|
    |7am|51|
    |8am|55|
    |9am|60|
    |10am|65|
    |11am|70|
    |noon|75|
    |1pm|80|

    a. Create a function that returns the current temperature based upon the time (0 hours=11am, 65$^{o}$F) 
    *Plot the function $T_a$ vs time. Does it look correct? Is there a better way to get $T_a(t)$?

    b. Modify the Euler approximation solution to account for changes in temperature at each hour. 
    Compare the new nonlinear Euler approximation to the linear analytical model. 
    At what time was the corpse 98.6$^{o}$F? i.e. what was the time of death?
```

```{code-cell} ipython3

```

```{code-cell} ipython3

```
