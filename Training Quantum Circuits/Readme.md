# Training Parameterized Quantum Circuits

## Where do we need to train parameterized circuits?
![image](https://user-images.githubusercontent.com/86155658/127032643-5bc04e52-d5e1-4d96-a695-11fbb2382df5.png)

## Our task
![image](https://user-images.githubusercontent.com/86155658/127032697-428adb9f-0c3b-4494-99cc-dd06adb9d06d.png)

## Gradients
![image](https://user-images.githubusercontent.com/86155658/127032732-d532e771-0ce6-4d61-a9c2-f42249c8dc97.png)

**_Computing expectation values_**
_With the ``qiskit.opflow`` module we can write and evaluate expectation values. The general structure for an expectation value where the state is prepared by a circuit is_

    expectation = StateFn(hamiltonian, is_measurement=True) @ StateFn(circuit)
    result = expectation.eval()
    
_The above code uses plain matrix multiplication to evaluate the expected value, which is inefficient for large numbers of qubits. Instead, we can use a simulator (or a real quantum device) to evaluate the circuits by using a ``CircuitSampler`` in conjunction with an expectation converter like ``PauliExpectation`` as_

    sampler = CircuitSampler(q_instance)  # q_instance is the QuantumInstance from the beginning of the notebook
    expectation = StateFn(hamiltonian, is_measurement=True) @ StateFn(circuit)
    in_pauli_basis = PauliExpectation().convert(expectation)
    result = sampler.convert(in_pauli_basis).eval()
    
    ### Finite difference gradients
    
    Arguably the simplest way to approximate gradients is with a finite difference scheme. This works independent of the function's inner, possibly very complex, structure.
    ![image](https://user-images.githubusercontent.com/86155658/127032977-fb77affe-8dff-4e75-8356-0775b5934d7e.png)

Finite difference gradients can be volatile on noisy functions and using the exact formula for the gradient can be more stable.
![image](https://user-images.githubusercontent.com/86155658/127033093-2f3d2bb5-79c3-43fd-affd-b48cb5427df6.png)

### Does this always work?
![image](https://user-images.githubusercontent.com/86155658/127033302-e433752c-c1c8-43de-ad90-2ed128bcaabb.png)

### Natural gradients
![image](https://user-images.githubusercontent.com/86155658/127033386-f74a90e9-fa12-45ba-a3f9-a9fc81a055b0.png)
![image](https://user-images.githubusercontent.com/86155658/127033429-ebd91ab3-0c85-46bc-b7f3-33f720116cc2.png)

### [Quantum Natural Gradient](https://arxiv.org/abs/1909.02108)
![image](https://user-images.githubusercontent.com/86155658/127033501-05c73b52-ad23-408e-a058-adaf7597f754.png)

### Simultaneous Perturbation Stochastic Approximation
Idea: avoid the expensive gradient evaluation by sampling from the gradients. Since we don't care about the exact values but only about convergence, an unbiased sampling should on average work equally well!
![image](https://user-images.githubusercontent.com/86155658/127033684-c49b2da4-f20a-4dcd-9cda-cc9f4f50b12e.png)

[Simultaneous Perturbation Stochastic Approximation of the Quantum Fisher Information](https://arxiv.org/abs/2103.09232)

## Training in practice
Today, evaluating gradients is really expensive and the improved accuracy is not that valuable since we have noisy readout anyways. Therefore, in practice, people often resort to using SPSA. To improve convergence, we do however not use a constant learning rate, but an exponentially decreasing one.
![image](https://user-images.githubusercontent.com/86155658/127033996-f711e4f1-2b56-4ac4-82f5-7b2a6d38f65a.png)
Qiskit will try to automatically calibrate the learning rate to the model if you don't specify the learning rate.

## How can you use it in VQC/QNNs? 
![image](https://user-images.githubusercontent.com/86155658/127034103-a8eddd9a-21de-4f6b-ad3c-44aa0386009d.png)

### Building a variational quantum classifier

Let's get the constituents we need:

* a Feature Map to encode the data
* an Ansatz to train
* an Observable to evaluate

## Limits in training circuits
We've seen that training with gradients works well on the small models we tested. But can we expect the same if we increase the number of qubits? To investigate that we measure the variance of the gradients for different model sizes. The idea is simple: if the variance is really small, we don't have enough information to update our parameters.

### Exponentially vanishing gradients (Barren plateaus)
[Barren plateaus in quantum neural network training landscapes](https://arxiv.org/abs/1803.11173)

[Cost Function Dependent Barren Plateaus in Shallow Parametrized Quantum Circuits](http://arxiv-export-lb.library.cornell.edu/pdf/2001.00550)
![image](https://user-images.githubusercontent.com/86155658/127034324-8afb1fee-ad59-404b-807d-8a42cd3ffda0.png)

## Summary
![image](https://user-images.githubusercontent.com/86155658/127034377-7bf3fa2f-b8b1-43fb-942c-f9cb7214b35d.png)

