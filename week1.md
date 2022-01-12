## Overview on current quantum ML algorithms

This week I've been focusing on literature reviews on current quantum machine learning techniques. Over the years, numerous quantum machine learning algorithms has been proposed covering several ML models. Because quantum circuits models has strictly unitary gates, all operations are linear. There are models that aims to revamp the training progress, [such as using Grover search instead of gradient descent](https://proceedings.neurips.cc/paper/2003/hash/505259756244493872b7709a8a01b536-Abstract.html). However, people haven't found a method that's suitable for near term quantum computers.

Another direction is to reproduce ML algorithms using a complete quantum approach, such as [QSVM](https://link.aps.org/doi/10.1103/PhysRevLett.113.130503), [Quantum Boltzmann machine](https://link.aps.org/doi/10.1103/PhysRevX.8.021050) and [QGAN](https://www.nature.com/articles/s41534-019-0223-2). Those methods have demonstrated exponential quantum advantage over traditional algorithms, but they usually require the data to be encoded in quantum space, which adds to the complexity of the algorithms and sometimes even takes away the speedup.

A third approach is to create a mapping between classical and quantum algorithms. This kind of mapping can be used to efficiently convert classical dataset to a quantum state to achieve speedup, such as this [mapping to quantum feature space for QSVM](https://link.aps.org/doi/10.1103/PhysRevLett.113.130503). It can also be used to [convert classical algorithms to quantum circuits](http://arxiv.org/abs/2006.14815) and leave training phase classical but make interfering phase quantum. This kind of approach is more suitable for current quantum hardware, but their use case tend to be specialized. For this project, a quantum feature map seems to be a more suitable candidate for it's relatively low hardware requirement.



[Return to overview](index.md)
