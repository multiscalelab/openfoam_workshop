# Tutorial 3 — 2D dam break with VOF

A column of water starts at rest in the lower-left corner of a square tank, open to the atmosphere at the top. Once released, gravity drives the water across the floor and over a rectangular obstacle. The tank is 0.584 m wide and 0.584 m high. The initial column is approximately 0.146 m wide and 0.292 m high.

OpenFOAM v2606 uses `interFoam` to solve the transient flow of water and air. The volume-of-fluid (VOF) method tracks the interface through `alpha.water`, the fraction of each cell occupied by water. A value of 1 means water, a value of 0 means air, and intermediate values represent cells crossed by the interface.

## Running the case

From the workshop directory, load OpenFOAM and run the case.

```bash
source /usr/lib/openfoam/openfoam2606/etc/bashrc
cd 03-2d-dam-break-vof
blockMesh
checkMesh
mkdir -p 0
cp 0.orig/U 0.orig/alpha.water 0.orig/p_rgh 0/
setFields
interFoam
```

After loading OpenFOAM, you can also run `./Allrun` from the case directory. `checkMesh` should report 2,268 cells and `Mesh OK.` The mesh has one cell layer along `z`, with an `empty` patch on the front and back faces. `setFields` sets the initial water column in 324 cells. The solver adjusts the time step using the flow and interface Courant numbers and writes results every 0.05 s up to 1 s.

## Visualizing the interface

```bash
paraFoam -builtin
```

In ParaView, click **Apply**, look along the `z` axis, and colour the domain by `alpha.water` with a colour range of 0 to 1. Play the animation from time 0 to see the column collapse, the wave strike the obstacle, and the resulting splash. To extract the interface, apply **Contour** to `alpha.water` at 0.5. If the field is available only as cell data, apply **Cell Data to Point Data** first.

## Case files

- `0.orig/` contains the initial and boundary conditions for velocity `U`, water fraction `alpha.water`, and pressure with the hydrostatic contribution removed, `p_rgh`. The run copies them to `0/` before `setFields` updates the water fraction.
- `constant/g` sets the gravitational acceleration. `constant/transportProperties` sets each phase's density and viscosity and the surface tension between water and air.
- `system/blockMeshDict` defines the five-block mesh and its boundary patches. `system/setFieldsDict` defines the initial water column.
- `system/controlDict` sets the simulation and output times. `system/fvSolution` includes the interface-compression and pressure–velocity coupling settings.

The case follows the classical dam-break geometry and uses the v2606 solver settings. [OpenFOAM v2606 reference case](https://gitlab.com/openfoam/core/openfoam/-/tree/OpenFOAM-v2606/tutorials/multiphase/interFoam/laminar/damBreak/damBreak) · [Wolf Dynamics Tutorial 3 slides](https://www.wolfdynamics.com/images/begtuts/T3_2D_dam_break.pdf)
