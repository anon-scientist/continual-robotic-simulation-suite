# CRoSS (Continual Robotic Simulation Suite)

**CRoSS** is a scalable benchmark suite for **Continual Reinforcement Learning (CRL)**, built on top of the **Gazebo** simulator and designed to study **forgetting**, **transfer**, and **scalability** in realistic robotic environments.

The suite provides both **physically simulated** and **kinematic** variants of robotic tasks, combining realism, reproducibility, and computational efficiency.
It enables researchers to evaluate continual learning algorithms on complex, embodied agents while maintaining full control over task diversity and training conditions.

This repository serves as the **entry point** to the CRoSS ecosystem and links to all benchmark- and experiment-specific repositories.

### Note on Repository Availability (Anonymous Submission)
*This repository and all dependend repositories are provided exclusively for anonymous review purposes.
To preserve anonymity during the submission process, the repositories are maintained in read-only mode and do not accept issues, pull requests, or discussions at this stage.
Upon acceptance of the paper, we will make the full public version of this project available under our official organization account.*
**Please do not share, redistribute, or reference this repository outside the review process.
We kindly ask reviewers to respect the anonymity requirements and keep all materials strictly confidential.**

## Key Features
* **Continual RL focus**: Benchmarks are explicitly designed for sequential task learning, measurement of catastrophic forgetting, and analysis of transfer.
* **Realistic robotic environments**: Based on Gazebo with physically simulated robots (e.g., 7-DoF arm, two-wheeled differential-drive robot), sensors, and collisions.
* **Kinematic variants for speed**: Non-simulated versions of several benchmarks use forward/inverse kinematics for 10–100x faster training while preserving the same interfaces.
* **Modular design**: Each benchmark lives in its own repository, with this main repo acting as a hub for documentation, links, and shared tooling.
* **Containerized setups**: Apptainer/Singularity setups support reproducible and "out-of-the-box" running CRL experiments on local machines and clusters.
* **ROS/Gymnasium compatibility**: Environments use Gazebo-Transport and can be bridged to ROS, and the environment manager mirrors the Gymnasium API for easy integration with existing RL code.
  
## Benchmark Repositories
Each benchmark or experiment is hosted in a dedicated repository.  
Use the links below to navigate to the environment you are interested in.

* [**Multi-task Line-Following (MLF)**](https://github.com/anon-scientist/cross-multi-task-line-following): two-wheeled differential-drive robot following colored lines with varied shapes, textures, and layouts.
* [**Multi-task pushing objects (MPO)**](https://github.com/anon-scientist/cross-multi-task-pushing-objects): two-wheeled robot pushing objects of different types and properties.
* [**High-Level Reaching (HLR)**](https://github.com/anon-scientist/cross-high-level-reaching): 7-DoF arm, cartesian-space control (DQN-based experiments).
* [**Low-Level Reaching (LLR)**](https://github.com/anon-scientist/cross-low-level-reaching): 7-DoF arm, joint-space control with episodic REINFORCE and sparse final reward.
* [**Kinematic High-Level Reaching (HLR-K)**](https://github.com/anon-scientist/cross-kinematic-high-level-reaching): Kinematic variant of the HLR benchmark, without the gazebo simulator.
* [**Kinematic Low-Level Reaching (LLR-K)**](https://github.com/anon-scientist/cross-kinematic-low-level-reaching): Kinematic variant of the LLR benchmark, without the gazebo simulator.

Each repository contains its own `README.md` with:
* installation instructions,
* environment description,
* and scripts for training and evaluation.

## Getting Started
For a more specific guide follow the instructions on the individual benchmarks `README.md`.
1. **Clone the benchmark repository** and any other, that the benchmark is dependent on.
2. **Clone the [ICRL](https://github.com/anon-scientist/cross-icrl) and [cl_experiments](https://github.com/anon-scientist/cl_experiment) repository** if not already done, which contains the RL-Framework that all the benchmarks require.
3. **Setup the Apptainer environment** with the `icrl.def` from the icrl repository.
4. When no other steps or requirements are in the benchmarks `README.md`: **Run the benchmark through the `main.bash`**.

## System Design and Integration
CRoSS is designed from the ground up to support reproducible and interchangeable experimentation across simulated, kinematic, and real-robot settings.

### Gymnasium-Compatible Environment Interface
All CRoSS environments implement an interface modeled directly after Gymnasium, including `reset()` ,`step(action)`,`perform_action()`, termination and truncation signals and standardized observation and reward outputs.
This makes CRoSS environments interchangeable with Gymnasium tasks with minimal effort, allowing researchers to plug our benchmarks into existing RL training pipelines with very little modifications.

### Unified Manager Layer for Simulation and Real Robots
Each benchmark uses a dedicated environment manager that handles Gazebo communication via `gz-transport` with command publishing and sensor subscription. 
It allows for synchronization between simulation and agent or alternatively, direct kinematic computation for fast non-simulated variants.
The manager is intentionally designed to be easily adaptable. The Gazebo communication layer can be swapped for a ROS bridge with only minimal changes.
This allows the same training code to be reused on physical robots, provided that the robot exposes compatible ROS topics/services.

### Separation of Concerns Through our Dedicated RL Framework
CRoSS uses our modular RL framework for Incremental Continual Reinforcement Learning (ICRL) a separate repository containing:
* the RL agent running all our experiments
* algorithm implementations (e.g. DQN)
* replay buffers (e.g. FIFO, prioritized)
* exploration strategies (e.g. epsilon-greedy)
* different neural network variants (e.g. DNN, GMM)

This seperation ensures identical pipelines across all benchmarks, full reproducibility of experiment configurations and consistent logging and evaluation.

### Containerized Execution
The [ICRL](https://github.com/anon-scientist/cross-icrl) Repository includes an Apptainer definition file that:
* installs Gazebo and Python dependencies
* ensures consistent runtime environments across machines
* enables reproducible execution on HPC clusters, servers, and personal workstations

Together, these components provide a reproducible, extensible, and hardware-compatible ecosystem for continual reinforcement learning research across simulation and real-world robotics.
