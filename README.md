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

---

# Roadmap

[x] 25/11:
  - ItemSpawner script 
  - Output conveyors
  
[ ] 26/11:
  - robot DH definition
  - robot modeling/ kitbashing
  
[] 27/11:
  - Robot inverse kinematics