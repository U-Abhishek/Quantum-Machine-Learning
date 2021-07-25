# Quantum Kernels and Support Vector Machines

# Data

The data we are going to work with today will be a small subset of the well known handwritten [digits dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#digits-dataset), which is available through scikit-learn. We will be aiming to differentiate between '0' and '1'. 

## Data Encoding

We will take the classical data and encode it to the quantum state space using a quantum feature map. The choice of which feature map to use is important and may depend on the given dataset we want to classify. Here we'll look at the feature maps available in Qiskit, before selecting and customising one to encode our data.

### Quantum Feature Maps

As the name suggests, a quantum feature map  is a map from the classical feature vector  to the quantum state. This is faciliated by applying the unitary operation on the initial state is the number of qubits being used for encoding.

The feature maps currently available in Qiskit ([`PauliFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.PauliFeatureMap.html), [`ZZFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.ZFeatureMap.html) and [`ZFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.ZZFeatureMap.html)) are those introduced in [_Havlicek et al_.  Nature **567**, 209-212 (2019)](https://www.nature.com/articles/s41586-019-0980-2), in particular the `ZZFeatureMap` is conjectured to be hard to simulate classically and can be implemented as short-depth circuits on near-term quantum devices.

### Exploration: Quantum Support Vector Classification

Try running Quantum Support Vector Classification with different Qiskit [feature maps](https://qiskit.org/documentation/apidoc/circuit_library.html#data-encoding-circuits) and [datasets](https://qiskit.org/documentation/machine-learning/apidocs/qiskit_machine_learning.datasets.html).


