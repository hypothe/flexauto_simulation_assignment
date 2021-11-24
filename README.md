# flexauto_simulation_assignment
 
# Setup notes

## Mesh import

For each of the three meshes of the desired object to work with a convex approximation has been generated using HACD (for **CamFhaft_Engine**) and V-HACD (for **FuelPump_Engine** and **OilTray_Engine** pieces, due to the inability of the former algorithm to model it in a satisfying way).
As a naming convention the convex shape has been named **<object-name>_dynamic** whilst the visual mesh **<object-name>_visual**: the latter has been set to non-respondable and non-dynamic, and placed in the hierarchy as the child of the convex shape.

Note that, in order to reduce the computational burden, convex shape have been set as non-renderable (among the *special properties*), whilst the visual shape as been set exclusively as renderable. This in order to have camera based system detect the actual visual shape of the object, which is instead not a priority in the other detection systems.

To all three shaped was appllied a material, in order to more accurately model their dynamic properties.