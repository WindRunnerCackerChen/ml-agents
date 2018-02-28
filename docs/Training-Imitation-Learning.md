# Imitation Learning 

It is often more intuitive to simply demonstrate the behavior we want an agent to perform, rather than attempting to have it learn via trial-and-error methods. For example, instead of training the medic by setting up its reward function, this mode allows providing real examples from a game controller on how the medic should behave. More specifically, in this mode, the Brain type during training is set to Player and all the actions performed with the controller (in addition to the agent observations) will be recorded and sent to the Python API. The imitation learning algorithm will then use these pairs of observations and actions from the human player to learn a policy.

## Using Behavioral Cloning

There are a variety of possible imitation learning algorithms which can be used, and for v0.3 we are starting with the simplest one: Behavioral Cloning. It works by collecting training data from an expert, and then simply uses it to directly learn a policy, in the same way the supervised learning for image classification or other traditional Machine Learning tasks work.

1. In order to use Behavioral Cloning in a scene, the first thing you will need is to create two Brains, one which will be the "Teacher," and the other which will be the "Student." We will assume that the names of the brain Gameobjects are "Teacher" and "Student" respectively.
2. Set the "Teacher" brain to Player mode, and properly configure the inputs to map to the corresponding actions. Ensure that "Broadcast" is checked within the Brain inspector window. 
3. Set the "Student" brain to External mode.
4. Link the brains to the desired agents (one agent as the teacher and at least one agent as a student).
5. Build the Unity executable for your desired platform.
6. In `trainer_config.yaml`, add an entry for the "Student" brain. Set the `trainer` parameter of this entry to `imitation`, and the `brain_to_imitate` parameter to the name of the teacher brain: "Teacher". Additionally, set `batches_per_epoch`, which controls how much training to do each moment.
7. Launch the training process with `python3 learn.py <env_name> --train --slow`, where `<env_Name>` is the name (or path to) your built Unity executable.
8. From the Unity window, control the agent with the Teacher brain by providing "expert demonstrations" of the behavior you would like to see.
9. Watch as the agent(s) with the student brain attached begin to behave similarly to the demonstrations.
10. Once the Student agents are exhibiting the desired behavior, end the training process with `CTL+C` from the command line.
11. Move the resulting `*.bytes` file into the `TFModels` sub-directory of the Assets folder (or a sub-directority within Assets of your choosing) , and use with `Internal` brain.
