# Parameter Learning using Approximate Model Counting

This repository contains the code that was used for the experiments of the paper ["Parameter Learning using Approximate Model Counting"](https://link.springer.com/chapter/10.1007/978-3-031-71170-1_9) published at NeSy in 2024. 

## Abstract of Paper

An emerging class of neurosymbolic methods relies on the use of neural networks to determine the parameters of symbolic probabilistic models. To train these hybrid models, these methods use a knowledge compiler to turn the symbolic model into a differentiable arithmetic circuit, after which gradient descent can be performed. However, these methods require compiling a reasonably sized circuit, which is not always possible, as for many symbolic probabilistic models calculating a gradient towards the parameters is #P-hard. We introduce a new approach for learning parameters using partially compiled circuits with approximation nodes. We show that, if the errors made in the approximation nodes are bounded, the error on the gradient of partially compiled circuits can also be bounded. We evaluate the impact of various approximation guarantees on this approach’s learning and generalization performance. Using approximation allows more complex queries to be compiled and our experiments show that their addition helps reduce the training loss. However, we observe that there is a limit to the addition of partial circuits after which there is no more improvement.

## Repository Structure

```
.
├── src/                # Schlandals solver source (Rust)
│   ├── solvers/
│       └── solver.rs    # Main contribution: partial compilation of arithmetic circuits
│   └── learning/
│       └── learner.rs   # Main contribution: learning with partially compiled ACs
├── pyschlandals/       # Python bindings for Schlandals (not used for this paper)
├── experiments/        # Scripts and notebooks used to produce the paper's results
│   ├── data/             # Input datasets and precomputed outputs
│   ├── exp1/             # What is the impact of the partial compilation on the convergence?
│   ├── exp2/             # How good is the learning process on unseen examples?
│   └── exp3/             # How does the number of queries used for training impact the learning? 
├── tests/               # Integration tests and CNF test instances
├── doc/                 # Schlandals documentation source (mdBook)
└── LICENSE
```

## Code

The code for the compilation of a partially compiled arithmetic circuit was originally implemented in a branch of the Schlandals solver, available and updated on the [Schlandals repository](https://github.com/aia-uclouvain/schlandals). For accesibility purposes, it is also included here, along with the code to run the experiments of the paper. The main contributions to the Schlandals code are located in `src/solver.rs` and `src/learning/learner.rs`. 

> **Note:** the version of Schlandals in this repository is a snapshot corresponding to the one used for the paper's experiments, and may differ from the latest version available in the main Schlandals repository.

## Installation

For installation instructions (Rust toolchain, system dependencies, build steps), please refer to the [Schlandals Installation Guide](https://aia-uclouvain.github.io/schlandals/install.html).

## Running the Experiments
All experiment code is located in the `experiments/` folder. Datasets were downloaded from the [BNLearn repository](https://www.bnlearn.com/bnrepository/) and are provided pre-processed in `experiments/data/`.

### Experiment 1 — What is the impact of the partial compilation on the convergence?

```bash
bash /experiments/exp1/bigaia_approx_sampling.sh # epsilon and delta are set to 0
bash /experiments/exp1/bigaia_approx_vary.sh # epsilon is set to 0.05, 0.3, 0.8
bash /experiments/exp1/bigaia_approx_delta.sh # epsilon and delta are set to 0.05, 0.3, 0.8 and 0.2, 0.5, 0.8 respectively
```

Results can be generated as a table in `table_epsilon.ipynb`.

### Experiment 2 — How good is the learning process on unseen examples?

```bash
bash experiments/bigaia_train_test.sh       
```

Results can be generated as a table in `table_train_test.ipynb`.

### Experiment 3 — How does the number of queries used for training impact the learning? 

```bash
bash experiments/bigaia_train_test_munin.sh       
```

Results can be generated as a table in `table_size.ipynb`.

## About Schlandals

[Schlandals](https://github.com/aia-uclouvain/schlandals) is a state-of-the-art *Projected Weighted Model Counter* specialized for probabilistic inference over discrete probability distributions. Currently, it supports modelization for the following problems:

- Computing the marginal probabilities of a variable in a Bayesian Network
- Computing the probability that two nodes are connected in a probabilistic graph
- Computing the probability of [ProbLog](https://github.com/ML-KULeuven/problog) programs

For more information on how to use Schlandals and its mechanics, check [the documentation](https://aia-uclouvain.github.io/schlandals).

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file included in this repository.

## Citing

If you use Schlandals, please cite:

```bibtex
@InProceedings{schlandals,
  author    = {Dubray, Alexandre and Schaus, Pierre and Nijssen, Siegfried},
  title     = {{Probabilistic Inference by Projected Weighted Model Counting on Horn Clauses}},
  booktitle = {29th International Conference on Principles and Practice of Constraint Programming (CP 2023)},
  year      = {2023},
  doi       = {10.4230/LIPIcs.CP.2023.15},
}
```

If you use the partially compiled arithmetic circuit method, please cite:

```bibtex
@InProceedings{dierckx2024parameter,
  author={Dierckx, Lucile and Dubray, Alexandre and Nijssen, Siegfried},
  title={Parameter Learning using Approximate Model Counting},
  booktitle={International Conference on Neural-Symbolic Learning and Reasoning},
  pages={80--88},
  year={2024},
  organization={Springer}
}
```