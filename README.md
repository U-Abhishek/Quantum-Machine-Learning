# Quantum-Machine-Learning

## 1)Quantum Computing Operations and Algorithms
### 1a.Quantum States and Circuits
#####     1: Bit Flip
#####     2: Plus State
#####     3: Minus State
#####     4: Complex State
#####     5: Bell State
#####     6: GHZ-like State
Check out this chapter if you would like to refresh the theory: https://qiskit.org/textbook/ch-gates/introduction.html.
### 1b.The Deutsch-Jozsa Algorithm
Many quantum algoritms revolve around the notion of so called oracles. An oracle is a function that can be considered as a 'black box'. We generally want to find out specific properties of this function. We do this by asking questions to the oracle (*querying*). The query complexity is then defined as the minimum number of queries in order to find these properties.


To get familiar with the use of oracles we will now consider the Deutsch-Josza problem. We will see that the quantum solution has a drastically lower query complexity than its classical counterpart.
#####     1: Classical Deutsch-Jozsa
#####     2: Quantum Deutsch-Jozsa

## 2)Variational Algorithms
###     1: MaxCut
Variational Quantum Eigensolvers
Variational quantum eigensolvers use the variational method to approximate the ground state and minimal eigenvalue of a Hamiltonian 𝐻. The trial state now corresponds to a quantum state prepared by a variational quantum circuit and the corresponding expectation value is measured by executing the circuit on a quantum computer. A classical optimizer is then used to tune the circuit parameters and minimize the measured expectation value.
![image](https://user-images.githubusercontent.com/86155658/127023231-6a419d9d-f20c-4567-bdd6-0d50c721ae7c.png)
Apart from being applicable to problems in chemistry or quantum mechanics itself, where we are directly interested in the ground state of a given Hamiltonian, one can also use the concept of variational quantum eigensolvers for optimization problems, by encoding the cost function that should be optimized as a Hamiltonian whose ground state corresponds to the optimal solution of the problem. This idea lies at the heart of QAOA.

###     2: MaxCut to Quadratic Program
In the MaxCut problem, the input is a (possibly weighted) graph and one attempts to find a partition of the graph vertices into two disjoint sets, such that the cumulative weight of edges connecting vertices from different cuts is maximized.
![image](https://user-images.githubusercontent.com/86155658/127024096-0c4ea65f-88c5-426e-8f1d-5517bf7a0243.png)

###     3: Quantum Approximate Optimization Algorithm
In this section, we will implement our own version of the QAOA variational form and solve the MaxCut problem defined in the sections above, using the Quantum Approximate Optimization Algorithm (QAOA) implemented in Qiskit. 
##### The QAOA and MinimumEigenOptimizer class
If you have managed to complete the previous exercises, congratulations! You have implemented your own version of QAOA that can solve general QUBO instances and in particular MaxCut problems by running on a quantum computer. Qiskit also provides its own implementation of QAOA which allows us to solve optimization problems with only a few lines of code. <br>
The [`QAOA`](https://qiskit.org/documentation/stubs/qiskit.algorithms.QAOA.html) class in Qiskit is located in `qiskit.algorithms` and directly inherits from the Variational Quantum Eigensolver class [`VQE`](https://qiskit.org/documentation/stubs/qiskit.aqua.algorithms.VQE.html). The initializer for `QAOA` takes the following input parameters, amongst others
- `optimizer`: This argument refers to the classical optimizer used for updating the circuit parameters. Qiskit offers a number of different [optimizers](https://qiskit.org/documentation/apidoc/qiskit.aqua.components.optimizers.html) and you can also define your own by subclassing Qiskit's native [`Optimizer`](https://qiskit.org/documentation/stubs/qiskit.aqua.components.optimizers.Optimizer.html#qiskit.aqua.components.optimizers.Optimizer) class. Some of the basic optimizers offered by Qiskit are the following: <br>
    - [COBYLA](https://qiskit.org/documentation/stubs/qiskit.aqua.components.optimizers.COBYLA.html#qiskit.aqua.components.optimizers.COBYLA): Constrained Optimization By Linear Approximation optimizer.
    - [SLSQP](https://qiskit.org/documentation/stubs/qiskit.aqua.components.optimizers.SLSQP.html#qiskit.aqua.components.optimizers.SLSQP): Sequential Least SQuares Programming optimizer.
    - [ADAM](https://qiskit.org/documentation/stubs/qiskit.aqua.components.optimizers.ADAM.html#qiskit.aqua.components.optimizers.ADAM):  A gradient-based optimization algorithm that is relies on adaptive estimates of lower-order moments.
- `reps`: The number of layers p in the QAOA variational form, i.e. the depth of the algorithm. For higher values of p, QAOA can theoretically obtain better results but the quantum circuit becomes deeper and there are more parameters to optimize.
- `initial_point`: Optional initial parameter values to start the optimization with.
- `quantum_instance`: The quantum instance to run the algorithm on. This can be a simulator or a real hardware device.
- `callback`: A callback function that can be used to monitor the optimization process.

Test the effect of some of these arguments on the optimization procedure of QAOA.
##### QAOA Energy Landscape
![newplot (3)](https://user-images.githubusercontent.com/86155658/127025478-8ba01799-1bfa-45ce-ab55-ad6f6960c338.png)

###     4: Conditional Value at Risk
#### CVaR
One recently proposed adaptation to QAOA is the idea of calculating the Conditional Value at Risk (or CVaR). Here, instead of calculating the mean of all cut values c_i obtained from the measurement outcomes of the circuit to compute the cost function f, the algorithm only takes into account a fraction alpha of the highest measured cut values.
Where the c_i are ordered decreasing in size.
Since we are only interested in obtaining a single good solution for the original optimization problem, looking only at a fraction of the best cuts can help to speed up the optimization process. Let us explore how adding CVaR can change the energy landscape of a QAOA instance. The following code creates and plots a QAOA energy landscape of a given MaxCut instance using your code from the previous exercises and returns the optimal parameters obtained during a grid search. Calculating the energy landscape might take a while.

![newplot (4)](https://user-images.githubusercontent.com/86155658/127026025-3d880b5c-1b16-4529-8306-3143988b0eca.png)

The algorithm calculates the conditional value at risk and observe how different settings of the parameter "alpha" change the QAOA energy landscape. What are the energy and optimal parameters we obtain by the grid search when setting the CVaR parameter 

![newplot (5)](https://user-images.githubusercontent.com/86155658/127026232-edd2d7eb-b621-4c6e-9da3-4d0837799341.png)

# Resources

- [Tutorial on VQEs](https://qiskit.org/textbook/ch-applications/vqe-molecules.html) in Qiskit Textbook
- [Tutorial on combinatorial optimization with QAOA](https://qiskit.org/textbook/ch-applications/qaoa.html) in Qiskit Textbook
- E. Farhi, J. Goldstone, and S. Gutmann, [A Quantum Approximate Optimization Algorithm (2014)](https://arxiv.org/abs/1411.4028): The original QAOA paper 
- [Qiskit Optimization Tutorials](https://qiskit.org/documentation/tutorials/optimization/index.html) including
    - [Quadratic Programs](https://qiskit.org/documentation/tutorials/optimization/1_quadratic_program.html)
    - [Quadratic Program Converters](https://qiskit.org/documentation/tutorials/optimization/2_converters_for_quadratic_programs.html)
    - [Minimum Eigen Optimizer](https://qiskit.org/documentation/tutorials/optimization/3_minimum_eigen_optimizer.html)
- Talk by E. Farhi on [recent results for QAOA](https://www.youtube.com/watch?v=8GnRRxlZuZo) at the Simons Institute for the Theory of Computing
