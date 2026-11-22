# Tutorial 2 — Laminar pipe flow

This case adapts `case0` from `laminar_pipe.tar.gz` to OpenFOAM v2606. The archive models flow between parallel plates in a 10 m long, 1 m high channel. The mesh has 100 × 10 cells in the `xy` plane and one layer along `z`, with `empty` front and back patches.

The fluid is initially at rest. The inlet velocity is 1 m/s in the `+x` direction, the top and bottom walls have no slip, and the outlet sets the reference kinematic pressure to zero. The kinematic viscosity is 0.01 m²/s, giving a Reynolds number of 100 based on the channel height and bulk velocity. `icoFoam` advances the solution to 20 s with a 0.05 s time step and writes the fields every 1 s.

## Running the case

From the workshop directory, load OpenFOAM and run the case.

```bash
source /usr/lib/openfoam/openfoam2606/etc/bashrc
cd 02-laminar-pipe
blockMesh
checkMesh
icoFoam
postProcess -func sampleDict -latestTime
```

After loading OpenFOAM, you can also run `./Allrun` from the case directory to execute the same four commands. `checkMesh` should report 1,000 cells and `Mesh OK.`

## Comparing with planar Poiseuille flow

At the final time, `system/sampleDict` samples the centreline and the cross-section at `x = 9 m`. The files in `postProcessing/sampleDict/20/` contain five columns in the order `coordinate`, `p`, `Ux`, `Uy`, `Uz`.

- In `downstreamProfile_p_U.xy`, plot `Ux` against `y` and compare it with `Ux(y) = 1.5 U_bulk [1 − (y/h)²]`, where `h = 0.5 m` is the channel half-height. The centreline speed should approach 1.5 m/s.
- In `centreline_p_U.xy`, plot `p` against `x`. The pressure falls more steeply near the inlet while the velocity profile develops. In the downstream region, the theoretical kinematic pressure gradient is `dp/dx = −3 ν U_bulk / h² = −0.12 m/s²`.

To see the developing velocity profile in ParaView, run `paraFoam -builtin`, click **Apply**, select time `20`, and colour the domain by `U → Magnitude`.
