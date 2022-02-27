# Week 8 Updates

The goal for this week is to expanding the size of the quantum network and start training on a real dataset. I implemented a hybrid neural network using PyTorch and Qiskit. There are several reasons to use the hybrid structure instead of using a pure quantum model: 

1. Current quantum hardware have only a small amount of qubits available (I have access to less than 7), which means small memory available for training, so a classical structure is needed preprocess and shrink down the size of the input before passing it to the neural layer.
2. The speed of quantum computers as well as simulations are both very slow. I implemented a single layer of QNN and it still took a very long time to train.
3. Quantum gates are all linear operations, so introducing non-linearly in the model is not a trivial thing to do. [There are ways to do this](https://www.nature.com/articles/s41467-020-14454-2), but it's computationally expansive and not very practical at this moment, so a classical model is needed for such tasks

My hybrid model looks like the following:

```python
HybirdNeuralNet(
  (conv1): Conv2d(1, 2, kernel_size=(5, 5), stride=(1, 1))
  (conv2): Conv2d(2, 16, kernel_size=(5, 5), stride=(1, 1))
  (dropout): Dropout2d(p=0.5, inplace=False)
  (fc1): Linear(in_features=256, out_features=64, bias=True)
  (fc2): Linear(in_features=64, out_features=2, bias=True)
  (qnn): TorchConnector()
  (fc3): Linear(in_features=1, out_features=1, bias=True)
)
```

here `qnn`is the quantum module written in `qiskit` and connected with their `pytorch_connector` API. The `qnn` layer is based on `OpFlowQNN` structure mentioned last week. For speed concerns, I only used 2 qubits in this specific model. The number of qubits is however easily changeable. The QNN operator looks like the following with 1024 shots:

```
ComposedOp([
  OperatorMeasurement(1.0 * ZZ),
  CircuitStateFn(
       ┌──────────────────────────┐┌──────────────────────────────────────┐
  q_0: ┤0                         ├┤0                                     ├
       │  ZZFeatureMap(x[0],x[1]) ││  RealAmplitudes(θ[0],θ[1],θ[2],θ[3]) │
  q_1: ┤1                         ├┤1                                     ├
       └──────────────────────────┘└──────────────────────────────────────┘
  )
])
```

The circuit diagram for `ZZFeatureMap` and `RealAmplitudes` are plotted below:

```
     ┌───┐┌─────────────┐                                          ┌───┐»
q_0: ┤ H ├┤ P(2.0*x[0]) ├──■────────────────────────────────────■──┤ H ├»
     ├───┤├─────────────┤┌─┴─┐┌──────────────────────────────┐┌─┴─┐├───┤»
q_1: ┤ H ├┤ P(2.0*x[1]) ├┤ X ├┤ P(2.0*(π - x[0])*(π - x[1])) ├┤ X ├┤ H ├»
     └───┘└─────────────┘└───┘└──────────────────────────────┘└───┘└───┘»
«     ┌─────────────┐                                          
«q_0: ┤ P(2.0*x[0]) ├──■────────────────────────────────────■──
«     ├─────────────┤┌─┴─┐┌──────────────────────────────┐┌─┴─┐
«q_1: ┤ P(2.0*x[1]) ├┤ X ├┤ P(2.0*(π - x[0])*(π - x[1])) ├┤ X ├
«     └─────────────┘└───┘└──────────────────────────────┘└───┘
```

```
     ┌──────────┐     ┌──────────┐
q_0: ┤ Ry(θ[0]) ├──■──┤ Ry(θ[2]) ├
     ├──────────┤┌─┴─┐├──────────┤
q_1: ┤ Ry(θ[1]) ├┤ X ├┤ Ry(θ[3]) ├
     └──────────┘└───┘└──────────┘
```

The dataset is trained on a largely reduced MNIST dataset, with 100 training samples from classes `0` and `1` to ensure a reasonable training time. 10 epochs of training took over an hour, but overall the model achieved over 99% test accuracy. Overall, the results look promising.

The goal for next week is trying to expand the size of the network and dataset and run the QNN layer on IBM's quantum computer. However, because 1. the quantum simulator is inefficient and it takes a very long times to run the network and 2. the IBM quantum computer usually have a high waiting time, I'm not sure if the training time of the network can be under one week on a real quantum computer.