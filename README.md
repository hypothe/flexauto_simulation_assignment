# Flexible Automation - Simulation Assignment

This folder *will present* the material related to the assignment of the simulation part of the Flexible Automation course at University of Genoa Robotics Engineering MSc. The goal of the assignment is to design and develop a manugacturing cell in which a robot segregates mixed items from an intake conveyor to three output conveyors.
 
# Setup notes

## Mesh import

For each of the three meshes of the desired object to work with a convex approximation has been generated using HACD (for **CamFhaft_Engine**) and V-HACD (for **FuelPump_Engine** and **OilTray_Engine** pieces, due to the inability of the former algorithm to model it in a satisfying way).
As a naming convention the convex shape has been named **<object-name>_dynamic** whilst the visual mesh **<object-name>_visual**: the latter has been set to non-respondable and non-dynamic, and placed in the hierarchy as the child of the convex shape.

Note that, in order to reduce the computational burden, convex shape have been set as non-renderable (among the *special properties*), whilst the visual shape as been set exclusively as renderable. This in order to have camera based system detect the actual visual shape of the object, which is instead not a priority in the other detection systems.

To all three shaped was appllied a material, in order to more accurately model their dynamic properties.

### Materials

item 			|	material 							| uniform density (g/cm^3)
---- 			| ---- 									| -----
Oil Tray 	| Cast Alluminium Alloy	| 2.71
Cam Shaft	| Cast Iron							|	7.20
Fuel Pump	| Cast Iron							|	7.20


## Item spawn

A dummy object (**ItemSpawner**, child of **InConveyor**) is responsible to spawn the items via its child script. The delay between each spawn is, currently, hardcoded as 20 seconds +- 4 seconds of random variability added on purpose.
To each model spawned is added a reasonable random rotation component to model variability in the incoming items orientation.

## Conveyors

Same width chosen to reduce the need to custom fit each one to each new item encountered, in order to increase generlizeability.

## Robot definition

Since the load/unload task does not require fine manipulation of the conveyed objects, a 3DoF Cartesian (PPP) mechanism has been chosen as the body of the robot. For the end effector, instead, has been articulated with a 2DoF (RR) system (yaw, pitch): the third degree has not been actuated, since the fact the items will always be presented on the conveyor belt reduces the manifold of possible gripping postures (or, at least, makes the reachable 2D manifold close enough to the optimum picking points to allow a firm grisp). This is further strenghtened by the end effector chosen.

## Actuation

Joint1: 
  - Type: prismatic
  - Min Position: 0.00 m
  - Range: 4.65 m
  - Max Force: 50.00 N
  - Upper Velocity Limit: 0.80 m/s
  - PID: (0.10, 0.00, 0.03)

Joint2: 
  - Type: prismatic
  - Min Position: 0.00 m
  - Range: 2.20 m
  - Max Force: 50.00 N
  - Upper Velocity Limit: 0.80 m/s
  - PID: (0.10, 0.00, 0.03)

Joint3: 
  - Type: prismatic
  - Min Position: 0.00 m
  - Range: 1.50 m
  - Max Force: 50.00 N
  - Upper Velocity Limit: 1.00 m/s
  - PID: (0.05, 0.00, 0.00)
  - 
Joint4: 
  - Type: revolute
  - Min Position: 0
  - Range: 360° (cyclic)
  - Max Torque: 2.50 N*m
  - Upper Velocity Limit: 15.00 deg/s
  - PID: (0.10, 0.00, 0.00)
  - 
Joint5: 
  - Type: revolute
  - Min Position: -120°
  - Range: 240°
  - Max Torque: 2.50 N*m
  - Upper Velocity Limit: 10.00 deg/s
  - PID: (0.10, 0.00, 0.00)

## Sensors

### Camera

  The idea is to use 2 RGB-D cameras with short field of view mounted on opposite ends of the robot structure, directly above the input conveyors and directed so as to look at the latter. RGB-D provide more information on the 3D rotation of the objects, which is especially useful to identify the correct grasping point for complex objects such as the Cam Shaft (for which the top-facing cylindrical piece should be grabbed in order to avoid collision with the item itself).
  While, ideally, a single RGB-D camera would have sufficed to keep the entire input-conveyor in the frame, it should have been placed in the center of the X axis of the mechanism in order for the sensor to have a top-down view of the belt (for a more reliable estimation of the items transformations). However, this configuration would have had the robotic arm always in the center of the frame, thus covering most of the belt.
  On the contrary, moving the camera to one of the edges would have caused a distortion that would have compromized the items' classification.
  The redundant configuration of the cameras here presented allows to always have a clear view of all items in at least one of the two frames. In terms of the computer vision algorithm to be used (here not presented) a robust object classification and tracking should be considered, that can manage to work with multiple sources, possibly overlapping and/or incomplete (due to the arm generating occlusion).

  > NOTE: since two visual sensors are used, we ccould opt for RGB ones with the depth information provided by stereo vision. However, due to the large distortion that the images would have (given the large inter-sensor distance and near focus point) this would probably be practically unfeasible.

  #### Technical Note

  As already mentioned, no actual CV algorithm is run in the simulation (since it's not the goal of this project): so, to retrieve the objects present in each camera frame, the sensor's render mode is set to *OpenGL, color coded handles*, where the information on the objects present in the field of view of the cameras are directly coded in the RGB values of the image. For this reason the resolution of the cameras has been kept low, since no real processing was computed. Keep in mind that, in the real scenario, this has to be carefully taken into consideration!

  > The input conveyor shape has been set to non-renderable to have the cameras ignore it.

  Similarly, the Depth value is not used here to detect the best gripping points among preset ones: the simplification adopted here is to just choose the picking point that's higher (larger world-frame z value), assuming that would always be the unencoumbered one.

---

# Roadmap

- [x] 25/11:
  - ItemSpawner script 
  - Output conveyors
  
- [x] 28/11:
  - Robot inverse kinematics

- [x] 30/11:
  - visual sensors placement
  - object detection

- [x] 01/12:
  - start working on FSM
    - identify the pick point of each incoming item
    - check if path can be modified at runtime
    - generate path toward item
  
- [x] 04/12:
  - abandon (temporarily) the work on the dynamic grab
  - modify the cell so as to stop the input conveyor once an item
    arrives at a predetermined grabbing point
  - should I re-evaluate the entire cell? Leave that to the end, if need be.
  - keep working on FSM
    - generate path to drop point
    - release item

- [x] 06/12:
  - robot DH definition
  
- [ ] 07/12:
  - object final orientation managing
  - object outout flow control

- [ ] ???/12
  - robot modeling/ kitbashing

## DEBUG

### PID definition

We should strive to remove vibration

Joint4:
  (0.05, 0, 0) small vibrations, slow
  (0.05, 0, 0.01) almost no vibrations, very slow