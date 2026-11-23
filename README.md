# Installing OpenFOAM and ParaView on Ubuntu (No sudo Required)

This guide shows how to install **OpenFOAM (OpenCFD v2412)** and **ParaView** on Ubuntu using Miniforge/Conda, without administrator (`sudo`) privileges.

All software will be installed in your home directory. No manual compilation is required.

## Requirements

- Ubuntu Linux, **x86-64** (`uname -m` should return `x86_64`)
- Internet access and a terminal
- Sufficient free disk space
- A graphical desktop to run ParaView

## 1. Install Miniforge

Download the installer:

```bash
cd ~
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
```

Install Miniforge in your home directory:

```bash
bash Miniforge3-Linux-x86_64.sh -b -p "$HOME/miniforge3"
```

Enable Conda in the current terminal:

```bash
source "$HOME/miniforge3/etc/profile.d/conda.sh"
conda --version
```

Optional: configure Conda for future Bash sessions (this changes only your personal `~/.bashrc`):

```bash
"$HOME/miniforge3/bin/conda" init bash
```

Open a new terminal after running `conda init`.

## 2. Install OpenFOAM

Create a Conda environment named `cfd`:

```bash
conda create -n cfd -c conda-forge openfoam=2412 -y
conda activate cfd
```

Verify the package and environment:

```bash
conda list openfoam
echo "$WM_PROJECT_VERSION"
which blockMesh
which icoFoam
which simpleFoam
```

The version variable should report `v2412`. **The `foamVersion` command is not required** and may be unavailable with this Conda package.

You can also view the information printed by an OpenFOAM utility:

```bash
blockMesh -help
```

## 3. Install ParaView

With the `cfd` environment active:

```bash
conda install -c conda-forge paraview -y
```

Verify and launch ParaView:

```bash
which paraview
paraview
```

If Conda reports incompatible dependencies, install ParaView in a **separate** environment instead:

```bash
conda create -n viz -c conda-forge paraview -y
conda activate viz
paraview
```

In that case, use the `cfd` environment for OpenFOAM simulations and the `viz` environment for visualisation.

## 4. Run Your First OpenFOAM Simulation

Activate the CFD environment if needed:

```bash
source "$HOME/miniforge3/etc/profile.d/conda.sh"
conda activate cfd
```

Create your personal working directory and copy the **lid-driven cavity** tutorial:

```bash
mkdir -p "$HOME/OpenFOAM/run"
cp -r "$CONDA_PREFIX/tutorials/incompressible/icoFoam/cavity/cavity" "$HOME/OpenFOAM/run/"
cd "$HOME/OpenFOAM/run/cavity"
```

> The Conda package installs tutorial files under `$CONDA_PREFIX/tutorials`. If the copy command fails, inspect available cases with `find "$CONDA_PREFIX/tutorials" -type d -name cavity`.

Generate and check the mesh:

```bash
blockMesh
checkMesh
```

Run the incompressible transient solver:

```bash
icoFoam
```

## 5. Visualise Results in ParaView

From the `cavity` case directory, create a marker file that ParaView can recognise:

```bash
touch cavity.foam
```

Open the case:

```bash
paraview cavity.foam
```

If ParaView is installed in the separate `viz` environment, switch to it first:

```bash
conda activate viz
paraview "$HOME/OpenFOAM/run/cavity/cavity.foam"
```

Inside ParaView:

1. Select the OpenFOAM reader if prompted, then click **Apply**.
2. Select the latest available simulation time.
3. Choose `U` and display **Magnitude** to visualise the velocity field.
4. Try the **Glyph**, **Slice**, or **Stream Tracer** filters.

## 6. Starting a New Terminal Session

If Conda is not automatically available, initialise it for the current terminal:

```bash
source "$HOME/miniforge3/etc/profile.d/conda.sh"
```

Then activate the environment:

```bash
conda activate cfd
```

To leave the environment:

```bash
conda deactivate
```

## Troubleshooting

### `conda: command not found`

```bash
source "$HOME/miniforge3/etc/profile.d/conda.sh"
conda activate cfd
```

### `foamVersion: command not found`

This is not necessarily an installation problem. Use:

```bash
echo "$WM_PROJECT_VERSION"
conda list openfoam
blockMesh -help
```

### `blockMesh: command not found`

```bash
conda activate cfd
conda list openfoam
which blockMesh
```

If `openfoam` is not listed, the package installation has not completed successfully.

### ParaView does not open

- Check that you are using a desktop session with a working graphical display.
- Check `which paraview` and `echo "$DISPLAY"`.
- If there are Conda dependency conflicts, use the separate `viz` environment described above.

### Cannot find the tutorials

```bash
echo "$CONDA_PREFIX"
ls "$CONDA_PREFIX/tutorials"
find "$CONDA_PREFIX/tutorials" -type d -name cavity
```

## Useful Commands

| Task | Command |
|---|---|
| Activate the CFD environment | `conda activate cfd` |
| Check the OpenFOAM version | `echo $WM_PROJECT_VERSION` |
| Generate a structured mesh | `blockMesh` |
| Check mesh quality | `checkMesh` |
| Run an incompressible transient simulation | `icoFoam` |
| Run a steady-state simulation | `simpleFoam` |
| Launch ParaView | `paraview` |
| Deactivate the environment | `conda deactivate` |

## Official Resources

- [Miniforge](https://github.com/conda-forge/miniforge)
- [OpenFOAM on conda-forge](https://anaconda.org/conda-forge/openfoam)
- [ParaView on conda-forge](https://anaconda.org/conda-forge/paraview)
- [OpenFOAM/OpenCFD documentation](https://www.openfoam.com/documentation/)
- [Official lid-driven cavity tutorial](https://www.openfoam.com/documentation/tutorial-guide/2-incompressible-flow/2.1-lid-driven-cavity-flow)

**Note:** This guide uses the conda-forge build of OpenFOAM OpenCFD **v2412**, not the Foundation distribution and not OpenCFD v2606. A graphical desktop is required for ParaView.
