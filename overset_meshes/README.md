# Workflow for setting up an overset mesh case
The following is a description of steps performed by the Allrun script.
1. Create background and foregroung meshes separately.
2. Merge meshes with foreground being the master region.
Note: The overset boundary must be defined first in the boundary file.
3. Renumber the mesh.
4. Run checkMesh to create cell sets: region0 - moving region, region1 - stationary region.
5. Run topoSet to create a cellZone for the moving region.
6. Create (copy) the "0" directory.
7. Run setFields to assign zoneIDs: 0 - background, 1 - foreground.
8. Define motion parameters (by hand).
9. Run moveDynamicMesh to check the mesh motion.

Notes on boundary and initial conditions:

U, p, turbulence - as usual except for the interface boundary which is of type overset

point displacement

  interface - patchType overset; type - zeroGradient;
  
  rotor - zeroGradient;
  
  everything else - fixed at 0;
  
zoneID

  interface - overset;
  
  everything else - zeroGradient;

