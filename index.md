# MoD-aware motion planning benchmark
- [MoD-aware motion planning benchmark](#mod-aware-motion-planning-benchmark)
  - [MoD-building repos](#mod-building-repos)
    - [Instructions and examples](#instructions-and-examples)
  - [MoD-planning repos](#mod-planning-repos)
    - [`mod_ros`](#mod_ros)
    - [`ompl_planners`](#ompl_planners)
    - [`mod_path_planning`](#mod_path_planning)
  - [Multi-agent coordination based Simulation](#multi-agent-coordination-based-simulation)

## MoD-building repos

1. CLiFF-map is available [here](https://github.com/tkucner/CLiFF-map-matlab).
2. STeF-map is available in the [`mod_ros`](https://github.com/ksatyaki/mod_ros) repository below.
3. Intensity-map and GMMT-map is available in the [`useful_scripts`](https://gitsvn-nt.oru.se/ksatyaki/useful_scripts) package.

### Instructions and examples
Some programs such as the `gmmtmap.py` in `useful_scripts` require that the input data file is in the ATC format (the same format used in the ATC dataset), but ordered by trajectory id.
The `useful_scripts` package contains many programs that help us convert files to the ATC format, or cut out pedestrian data at specific times, etc.
For example, the `split_into_segmets.py` is used as follows: 
 - Data generated from PedSim is available as a ROS-bag file. Let's call this `data1.bag`
 - This data is converted to ATC format using `spencer_rosbag_to_atc.py` &rarr; data1.csv
 - We need to order this data using the `id` of the object. So we run `order_by_id.py` &rarr; data1_ordered.csv
 - PedSim tracks all its pedestrians continuously, so we can set a border (outside which we pretend that PedSim loses the pedestrian) and use this border to split the different trajectories appropriately.
 - For this we run `split_into_segments.py` that produces the desired result. 

Notice that the above example is only if we need to generate a GMMT-map from the data. 
   

## MoD-planning repos

### [`mod_ros`](https://github.com/ksatyaki/mod_ros)

- ROS packages that read and publish different MoD maps to ROS services. 
- ROS Messages and Service support for GMMT-map and Intensity-map (available [here](https://gitsvn-nt.oru.se/ksatyaki/useful_scripts) under `atc_dataset`).
- ROS Messages and Service support for CLiFF-map (available [here](https://github.com/tkucner/CLiFF-map-matlab)).
- ROS Messages and Service support for STeF-map available in this repository including ROS interfaces.

### [`ompl_planners`](https://github.com/ksatyaki/ompl_planners)

- Libraries providing Optimization Objectives (ompl) based on all the MoDs in `mod_ros`.
- This is used in conjunction with the `mod_path_planning`

### [`mod_path_planning`](https://github.com/ksatyaki/mod_path_planning)

- Contains pre-built MoDs and their occupancy maps for certain environments.
- Planner executable and launch files. 
- Please look at the `launch` directory and especially the files: `planner_pedsim_warehouse.launch` and `planner_atc.launch`.
- Please also look at the `tmule` directory, and especially [this file](https://github.com/ksatyaki/mod_path_planning/blob/master/tmule/tests-all.yaml). 
- We recommend using [TMuLE](https://github.com/marc-hanheide/TMuLE). It's super easy to install via pip and makes life a lot easier. 
- The TMuLE scripts basically tell tmux to launch certain scripts/programs in certain tmux windows and tabs. So these are all the programs one needs to start in order to start the motion planning. 
- The last window in the tmule yaml file is the list of testing scripts (these post messages via ROS with the start state, goal state, planning time, etc.).


## Multi-agent coordination based Simulation

- Repository available [here](https://github.com/FedericoPecora/coordination_oru).
- For our simulation we need the `uncontrollable-robots-pedestrians` branch!
- Take a look at the [directory here](https://github.com/FedericoPecora/coordination_oru/tree/uncontrollable-robots-pedestrians/src/main/java/se/oru/coordination/coordination_oru/tests/testsPedestrians)
- It contains the `MultiplePedestriansAndRobot.java` file and other scripts that make it easier to run several tests together. :-)
- Don't forget to contact me via github if you have any questions! 
