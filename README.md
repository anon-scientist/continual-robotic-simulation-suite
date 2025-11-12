# CRoSS (Continual Robotic Simulation Suite)

CRoSS is a scalable benchmark suite for Continual Reinforcement Learning (CRL), built on top of the Gazebo simulator and designed to study forgetting, transfer, and scalability in realistic robotic environments.
The benchmark provides both physically simulated and kinematic variants of robotic tasks, combining realism, reproducibility, and computational efficiency.
It enables researchers to explore continual learning algorithms on complex, embodied agents while maintaining full control over task diversity and training conditions.

## Project Setup

**Make sure that you have successfully set up Gazebo!**

To setup the project run the following bash script:

```bash
./setup-project.sh
```

## Start Simulation

To start the simulation run the following bash script:

```bash
./start-simulation.bash
```

## Inverse Kinematics

### Set Target Position

Set the target position in `simulation/IKPanda.py`

### Calculate Inverse Kinematics

To calculate the inverse kinematics for the specified target position and then move the robot arm in Gazebo to the target position using the calculated IK solution, execute the Python file:

```bash
python3 simulation/IKPanda.py
```


## Important Gazebo Topics

For the following commands the somulation must be running!

Get list of all available topics:

```bash
gz topic -l
```

Subscribe to world dynamic-pose info topic:
```bash
gz topic -e -t /world/test-world/dynamic_pose/info
```

Subscribe to panda-arm joints state:
```bash
gz topic -e -t /model/panda_arm/joints/state
```

Get message type for panda-arm joint topics (example for joint1):
```bash
gz topic --info --topic /model/panda_arm/joint/panda_joint1/0/cmd_pos
```

## Test World

![alt text](image.png)

## Commands

### Get Topics

```bash
gz topic -l
```

## Blender SDF Export

With the `Blender/sdf_exporter.py` script, Blender models can be exported as an sdf file and then integrated into the gazebo world.

## Continual World Paper

### Action Space

The action space is 4-dimensional and describes the desired changes to the robot arm and gripper:

Components of the action:
1. 3D change of the position of the end effector:
   - Changes in x-, y- and z-direction.
   - Describes where the end effector should move to in the next step.
2. Gripper modification (gripper):
    - A single value that describes the opening or closing of the gripper.
    - Positive value: Open.
    - Negative value: Close.

### Observation Space

The Observation Space is 12-dimensional and covers the most important states of the environment:

Components of the observation:
1. 3D position of the robot end effector:
   - The end effector is the tool or hand of the robot that performs the tasks.
   - In this case, it is the position of the “hand” of the robot arm in Cartesian space (x, y, z).
2. 3D position(s) of the objects:
   - The position(s) of the objects with which the robot should interact.
   - This includes 3 values per object, a total of 6 values for up to 2 objects.
3. 3D position of the target:
   - The target position that the object is to reach, also in Cartesian space (x, y, z).
