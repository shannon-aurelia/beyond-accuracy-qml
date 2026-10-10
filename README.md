# Beyond Accuracy: Benchmarking QML Against Classical Baselines

I started this project after reading more about quantum advantage and realizing that there is a big difference between saying:

> "a quantum model works"

and saying:

> "the quantum part is actually giving us an advantage."

A lot of QML examples focus on whether a quantum model can reach competitive accuracy, but I became more interested in what happens when the classical baseline is treated seriously and the cost of the quantum model is counted too.

This project is my first small benchmark around that question.

## What I am testing

I compare a small Variational Quantum Classifier (VQC) against a few classical models on the same binary Iris classification task.

The models are:

- Logistic Regression
- RBF SVM
- Small MLP
- 4-qubit Variational Quantum Classifier

The goal is not to prove quantum advantage from Iris. Iris is obviously a very small classical dataset.

I am using it first because it has four features, which maps naturally to a four-qubit circuit and makes it easy to test the benchmarking pipeline without introducing PCA or a larger preprocessing stack.

For the classification task, I use:

- Iris versicolor
- Iris virginica

I chose this instead of setosa vs the rest because the classes are less trivially separable.

## Why compare more than accuracy?

If two models both get similar accuracy, that does not necessarily mean they are equally useful.

For the VQC, I also track things such as:

- training time
- number of trainable parameters
- quantum circuit evaluations
- finite-shot behavior

The reason is simple: if the quantum model only matches a classical model after much more computational effort, accuracy alone does not tell the whole story.

## Quantum model

The VQC currently uses:

- 4 qubits
- angle encoding
- trainable `RY` rotations
- CNOT entangling gates
- Pauli-Z expectation value as the output
- Adam as the classical optimizer

The workflow is roughly:

```text
classical data
    ↓
standardization
    ↓
angle encoding
    ↓
quantum circuit
    ↓
measurement
    ↓
classical optimization