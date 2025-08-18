## Project Overview
The main objective of this repository is to explore the potential of QML for predictive modeling by developing a hybrid quantum-classical solution. This project aimed to:

- Implement a hybrid quantum neural network layer within a classical deep learning framework (TensorFlow).

- Develop a custom quantum dropout ansatz mechanism to enhance model generalization.

- Analyze the impact of different quantum layer architectures and qubit counts on model performance.

The project uses the Abalone dataset for regression, predicting the number of rings based on physical measurements.

## Technical Details
Hybrid Model Architecture
The model is a hybrid neural network consisting of both classical and quantum layers:

- Classical Input Layer: A standard classical layer for processing input features.
- Classical Embedding Layer: A standard classical layer to downscale or upscale the number of features.
- Quantum Layer: A custom-built quantum layer responsible for embedding classical data into qubits, performing quantum operations, dropping parameters based on a dropout rate and making measurements.
- Classical Dense Layer: A classical layer to process the quantum measurements
- Classical Output Layer: A final classical layer to produce the final regression prediction.

## Quantum Layer Implementation
The quantum layer is implemented using the PennyLane framework, integrated with TensorFlow. It uses the default.qubit simulator, which provides a perfect, noise-free quantum simulation environment. This simulation is a good representation of future fault-tolerant quantum computers (FTQC), which are expected to have fidelities approaching 100%.

The core of the quantum layer is a QNode that performs three main steps:

- Data Embedding: Classical features are encoded into qubits using an angle embedding or amplitude embedding strategy. Angle embedding applies rotation gates (Rx, Ry, Rz) to the qubits, with their angles determined by the input features. Each qubit can encode a single feature. Amplitude embedding, on the other hand, encodes features into the amplitudes of the qubits, allowing an exponential number of features (2<sup>n</sup>, where n is the number of qubits) to be encoded.

- Ansatz: The sequence of quantum gates with tunable parameters. The goal is to prepare a specific quantum state or to explore the quantum state space to find the best parameters for a problem. A key feature of the ansatz is quantum dropout, which is implemented as a custom operation that selectively "drops" some of the ansatz's rotation parameters during training.

    - StronglyEntangling: This ansatz utilizes all three rotation gates: R<sub>X</sub>, R<sub>Y</sub>, and R<sub>Z</sub>. It has a more complex entangling structure designed for more effective exploration of the Hilbert space.

    - BasicEntangler: This ansatz only uses the R<sub>Y</sub> rotation gate. It connects each qubit to the one that follows it in a simpler entangling strategy.

- Measurements: After the operations, measurements are performed on the qubits. The results of these measurements, which capture the state and correlations of the qubits, serve as the output of the quantum layer.
