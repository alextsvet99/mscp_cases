# OpenFOAM cases for MSc project

This repository stores the main test cases which were used in my MSc project. The cases can be used as a tutorial for setting up an overset meshes case and a morphing mesh case.

## overset_meshes

This case demonstrates the set up of overset meshes. Both rotation and surge are included.

## steady_rotation_isolated

This case demonstrates the base case where only turbine rotation is considered. The rotaton is implemented using a sliding mesh interface (AMI in OpenFOAM terms).

## steady_rotation_inWake

This case considers only rotation similarly to the previous case, but inlet and outlet boundaries are modelled as cyclic boundaries to create a sort of infinite array of turbines.

## morphing_mesh_isolated

This case models both rotation and surge. The rotaton is implemented using the sliding mesh interface, while the surging motion is implemented with a morhping mesh approach.

## morphing_mesh_inWake

This case considers both rotaion ans surge similarly to the previous case, but inlet and outlet boundaries are modelled as cyclic boundaries to create a sort of infinite array of turbines.

## Workflow for setting up an overset mesh case

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
* U, p, turbulence - as usual except for the interface boundary which is of type overset
* point displacement
  - interface - patchType overset; type - zeroGradient;
  - rotor - zeroGradient;
  - everything else - fixed at 0;
* zoneID
  - interface - overset;
  - everything else - zeroGradient;

## Workflow for setting up a surging AMI case

The following is a general description of steps needed to combine AMI and morphing mesh.

1. Create meshes for rotating and stationary regions separately.
2. Merge meshes with mergeMesh. Order of merging (master/slave) doesn't seem to matter.
3. Renumber mesh.
4. Create AMIs with createPatch. Order of AMI patches (master/slave) doesn't seem to matter.
5. Run checkMesh to create cellSets.
6. Create all sets and zones:
- faceZone fot translation faces - for defining surge;
- cellZone for rotating region - for defining rotation and surge;
- cellZone for stationary region - for defining deformation;
- faceSet for all AMI faces - needed for decomposition constraints;
7. Define motions in dynamicMeshDict.
8. Run moveDynamicMesh -noFunctionObjects -checkAMI.

Notes:
