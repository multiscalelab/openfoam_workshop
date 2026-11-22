# Tutorial 1 — Lid-driven cavity

The fluid starts at rest in a square cavity with a side length of 0.1 m. The top lid moves at 1 m/s in the `+x` direction, and the other three walls are stationary with no-slip boundary conditions. The case uses `icoFoam` in OpenFOAM v2606 to simulate transient, laminar, incompressible flow. The kinematic viscosity is 0.01 m²/s, giving a Reynolds number of `Re = U L / ν = 10`.

## Running the case

From the workshop directory, load OpenFOAM v2606 and run the case. The first command uses the local installation path.

```bash
source /usr/lib/openfoam/openfoam2606/etc/bashrc
cd 01-lid-driven-cavity
blockMesh
checkMesh
icoFoam
```

`blockMesh` generates a uniform mesh of 20 × 20 cells in the `xy` plane with a single layer along `z`. The front and back faces use the `empty` patch type to make the case two-dimensional. `checkMesh` validates the mesh and should report `Mesh OK.` The solver advances to 0.5 s in steps of 0.005 s and writes the fields every 0.1 s.

To run the same three commands in sequence, execute `./Allrun` from the case directory after loading OpenFOAM.

## Visualizing the results

```bash
paraFoam -builtin
```

In ParaView, click **Apply**, select the final time `0.5`, and colour the domain by `U → Magnitude`. Look for the high-speed region just below the lid. To see the clockwise vortex, view the `xy` plane from `+z` and use the **Glyph** filter with `U` as the orientation vector.

## Case files

- `0/U` and `0/p` set the initial and boundary conditions for velocity and kinematic pressure.
- `constant/transportProperties` sets the kinematic viscosity `nu`.
- `system/blockMeshDict` defines the mesh and its boundaries.
- `system/controlDict` sets the time-stepping and output intervals.
- `system/fvSchemes` and `system/fvSolution` configure the numerical schemes and the PISO algorithm.

Official reference case. [OpenFOAM v2606 — cavity](https://gitlab.com/openfoam/core/openfoam/-/tree/OpenFOAM-v2606/tutorials/incompressible/icoFoam/cavity/cavity)
