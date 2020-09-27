# QOSF Mentorship Screening Task 2

My answer can be found in learn-bell.ipynb. Below is a short write-up which is also included in that jupyter notebook where the equations are properly rendered.

### Author
Tony Tong 

### Question
Task 2
Implement a circuit that returns |01> and |10> with equal probability.
Requirements :
The circuit should consist only of CNOTs, RXs and RYs. 
Start from all parameters in parametric gates being equal to 0 or randomly chosen. 
You should find the right set of parameters using gradient descent (you can use more advanced optimization methods if you like). 
Simulations must be done with sampling (i.e. a limited number of measurements per iteration) and noise. 

Compare the results for different numbers of measurements: 1, 10, 100, 1000. 

#### Bonus
How to make sure you produce state |01> + |10> and not |01> - |10> ?

(Actually for more careful readers, the “correct” version of this question is posted below:
How to make sure you produce state  |01⟩  +  |10⟩  and not any other combination of |01> + e(i phi)|10⟩ (for example |01⟩  -  |10⟩)?)

## Answer Summary

### Introduction
In a general 2-qubit system
$$|\psi\rangle=\alpha|00\rangle+\beta|01\rangle+\gamma|10\rangle+\delta|11\rangle$$

$$\left|\beta\right|^{2} = \left|\gamma\right|^{2} = 0.5$$


So in order to satisfy $\left|\beta\right|^{2} = \left|\gamma\right|^{2} = 0.5$:

$$q_1 = | 0 \rangle, q_2 = | 1 \rangle$$ $$q_1 = | 1 \rangle, q_2 = | 1 \rangle$$

When measuring in Z basis:
$$\langle 0| \sigma_z  | 0 \rangle = 1$$
$$\langle 1| \sigma_z  | 1 \rangle = -1$$


### Parametrized Models
Three parametrized quantum circuits (PQC) with different optimization methods, namely Rotosolve, Rotoselect [1] and QGAN [2], will be explored.

#### Rotosolve
Rotosolve will optimize a given parameters for a given circuit ansatz. The circuit ansatz chosen is made of 2 layers of single qubit Pauli-Y/X rotation and a CNOT block to entangle the 2 qubits so that it is expressible enough while having resonable noise.

#### Rotoselect
Rotoselect builds on the Rotosolve algorithm, now the choice of the parametric gates themselves (Rx, Ry in our case) can also be optimized.

#### Loss fucntion for Rotosolve and Rotoselect
Since $q_1, q_2$ cancels each other in Z basis, we can choose our loss function for the opimizing algorithms:
$$L = |\langle q_1| \sigma_z  | q_1 \rangle + \langle q_2| \sigma_z  | q_2 \rangle|$$

#### QGAN

### Metric
To evaluate the accuracy and the stability of the optimization methods, two metrics will be used:
1. The standard deviation of loss after 10 iterations
2. Mean squared error = $\frac{1}{2}[(\left|\beta\right|^{2} - 0.5)^2 + (\left|\gamma\right|^{2} - 0.5)^2]$

Note the second metric access the information in the state vector directly and thus is only possible to get in simulation.


### References
[1] M. Ostaszewski, E. Grant, and M. Benedetti, Quantum Circuit Structure Learning, ArXiv:1905.09692 [Quant-Ph] (2019).

[2] C. Zoufal, A. Lucchi, and S. Woerner, Quantum Generative Adversarial Networks for Learning and Loading Random Distributions, Npj Quantum Inf 5, 103 (2019).
