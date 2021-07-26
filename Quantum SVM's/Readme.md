# Quantum Kernels and Support Vector Machines

### Data

The data we are going to work with today will be a small subset of the well known handwritten [digits dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#digits-dataset), which is available through scikit-learn. We will be aiming to differentiate between '0' and '1'. 
![image](https://user-images.githubusercontent.com/86155658/127029264-eddcebc7-ce78-4dc2-99a9-a9b85796638e.png)


### Data Encoding

We will take the classical data and encode it to the quantum state space using a quantum feature map. The choice of which feature map to use is important and may depend on the given dataset we want to classify. Here we'll look at the feature maps available in Qiskit, before selecting and customising one to encode our data.

### Quantum Feature Maps

As the name suggests, a quantum feature map  is a map from the classical feature vector  to the quantum state. This is faciliated by applying the unitary operation on the initial state is the number of qubits being used for encoding.

The feature maps currently available in Qiskit ([`PauliFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.PauliFeatureMap.html), [`ZZFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.ZFeatureMap.html) and [`ZFeatureMap`](https://qiskit.org/documentation/stubs/qiskit.circuit.library.ZZFeatureMap.html)) are those introduced in [_Havlicek et al_.  Nature **567**, 209-212 (2019)](https://www.nature.com/articles/s41586-019-0980-2), in particular the `ZZFeatureMap` is conjectured to be hard to simulate classically and can be implemented as short-depth circuits on near-term quantum devices.

### Quantum Kernel Estimation
As discussed in [*Havlicek et al*.  Nature 567, 209-212 (2019)](https://www.nature.com/articles/s41586-019-0980-2), quantum kernel machine algorithms only have the potential of quantum advantage over classical approaches if the corresponding quantum kernel is hard to estimate classically. 

As we will see later, the hardness of estimating the kernel with classical resources is of course only a necessary and not always sufficient condition to obtain a quantum advantage. 

However, it was proven recently in [*Liu et al.* arXiv:2010.02174 (2020)](https://arxiv.org/abs/2010.02174) that learning problems exist for which learners with access to quantum kernel methods have a quantum advantage over allclassical learners.

With our training and testing datasets ready, we set up the `QuantumKernel` class with the [ZZFeatureMap](https://qiskit.org/documentation/stubs/qiskit.circuit.library.ZZFeatureMap.html), and use the `BasicAer` `statevector_simulator` to estimate the training and testing kernel matrices.
This process is then repeated for each pair of training data samples to fill in the training kernel matrix, and between each training and testing data sample to fill in the testing kernel matrix. Note that each matrix is symmetric, so to reduce computation time, only half the entries are calculated explictly.

##### Training and testing kernel matrices:
![image](https://user-images.githubusercontent.com/86155658/127029948-4f1b2268-0141-4119-9eca-3c7ae01809a8.png)

### Quantum Support Vector Classification
Introduced in Havlicek et al. Nature 567, 209-212 (2019), the quantum kernel support vector classification algorithm consists of these steps:
![image](https://user-images.githubusercontent.com/86155658/127030119-455cbdad-af97-4213-b0d1-e05ebaa36c45.png)
1)Build the train and test quantum kernel matrices.

2)Use the train and test quantum kernel matrices in a classical support vector machine classification algorithm.

The `scikit-learn` `svc` algorithm allows us to define a [custom kernel](https://scikit-learn.org/stable/modules/svm.html#custom-kernels) in two ways: by providing the kernel as a callable function or by precomputing the kernel matrix. We can do either of these using the `QuantumKernel` class in Qiskit.

### Exploration: Quantum Support Vector Classification

Try running Quantum Support Vector Classification with different Qiskit [feature maps](https://qiskit.org/documentation/apidoc/circuit_library.html#data-encoding-circuits) and [datasets](https://qiskit.org/documentation/machine-learning/apidocs/qiskit_machine_learning.datasets.html).


