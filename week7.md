# Week 7 Updates

This week, I have been working on designing the backward pass of the quantum neural network using parameter shift rule as well as learning Qiskit Machine Learning APIs. I constructed 2 toy networks using different models using Qiskit, namely OpFlow and Circuit QNN. The first uses creates a complex operators such as the observable of a quantum state. An example is given below:

```
ComposedOp([
  OperatorMeasurement(2.0 * X
  + 1.0 * Z),
  CircuitStateFn(
      ┌───┐┌───────┐┌───────┐
  q0: ┤ H ├┤ Rz(a) ├┤ Rx(b) ├
      └───┘└───────┘└───────┘
  )
])
```

Qiskit's gradient framework can be used to calculate the gradient of the OpFlow Network using parameter-shift. A sample gradient is the following:

```
ListOp([
  SummedOp([
    ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌─────────────┐┌───────┐┌───┐
      q0: ┤ H ├┤ Rz(a + π/2) ├┤ Rx(b) ├┤ H ├
          └───┘└─────────────┘└───────┘└───┘
      )
    ]),
    -1.0 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌─────────────┐┌───────┐┌───┐
      q0: ┤ H ├┤ Rz(a - π/2) ├┤ Rx(b) ├┤ H ├
          └───┘└─────────────┘└───────┘└───┘
      )
    ]),
    0.5 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌─────────────┐┌───────┐
      q0: ┤ H ├┤ Rz(a + π/2) ├┤ Rx(b) ├
          └───┘└─────────────┘└───────┘
      )
    ]),
    -0.5 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌─────────────┐┌───────┐
      q0: ┤ H ├┤ Rz(a - π/2) ├┤ Rx(b) ├
          └───┘└─────────────┘└───────┘
      )
    ])
  ]),
  SummedOp([
    ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌───────┐┌─────────────┐┌───┐
      q0: ┤ H ├┤ Rz(a) ├┤ Rx(b + π/2) ├┤ H ├
          └───┘└───────┘└─────────────┘└───┘
      )
    ]),
    -1.0 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌───────┐┌─────────────┐┌───┐
      q0: ┤ H ├┤ Rz(a) ├┤ Rx(b - π/2) ├┤ H ├
          └───┘└───────┘└─────────────┘└───┘
      )
    ]),
    0.5 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌───────┐┌─────────────┐
      q0: ┤ H ├┤ Rz(a) ├┤ Rx(b + π/2) ├
          └───┘└───────┘└─────────────┘
      )
    ]),
    -0.5 * ComposedOp([
      OperatorMeasurement(Z),
      CircuitStateFn(
          ┌───┐┌───────┐┌─────────────┐
      q0: ┤ H ├┤ Rz(a) ├┤ Rx(b - π/2) ├
          └───┘└───────┘└─────────────┘
      )
    ])
  ])
])
```

The Circuit QNN uses quantum circuit as a base and uses similar gradient method for backward pass.

## Milestone Update

**Goal**: The goal for second milestone is to build a working network consists of both forward and backward passes. 

**Progress**: I have largely accomplished the goal. I have built a working trainable Quantum network as a proof of concept. Although the current model consists only a single qubit, the network is not very difficult to scale up. It will probably only require 4 qubits for basic classification tasks. I found the `Qiskit Machine Learning` package which provides convenient APIs for forward and backward passes as well as good integration with PyTorch. 