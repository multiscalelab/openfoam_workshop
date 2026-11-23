# **Installing OpenFOAM on Ubuntu Without Administrator Privileges**

This guide explains how to install **OpenFOAM (OpenCFD distribution)** on Ubuntu without `sudo` privileges using **Miniforge and Conda**.

The installation is performed entirely within your home directory and does not require compiling OpenFOAM from source.

**Requirements:**

- Ubuntu Linux (x86-64)
- Internet connection
- Terminal access
- No administrator privileges required

## **Step 1 — Download Miniforge**

Open a terminal and download the Miniforge installer:

```
cd ~

wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
```

## **Step 2 — Install Miniforge**

Install Miniforge in your home directory:

```
bash Miniforge3-Linux-x86_64.sh -b -p "$HOME/miniforge3"
```

Activate Conda for the current terminal session:

```
source ~/miniforge3/etc/profile.d/conda.sh
```

Verify the installation:

```
conda --version
```

You should see the installed Conda version.

## **Step 3 — Install OpenFOAM**

Create a new Conda environment called `cfd` and install OpenFOAM:

```
conda create -n cfd -c conda-forge openfoam=2412 -y
```

Activate the environment:

```
conda activate cfd
```

This installs **OpenFOAM OpenCFD v2412** using the conda-forge package.

## **Step 4 — Verify the Installation**

Check the OpenFOAM version:

```
foamVersion
```

Check whether the main utilities and solvers are available:

```
which icoFoam
which simpleFoam
which blockMesh
```

Locate the tutorial directory:

```
echo $FOAM_TUTORIALS
```

The `which` commands should return the paths of the corresponding executables. If they do not, the Conda package's environment configuration may need to be checked.

## **Step 5 — Configure Conda for Future Sessions**

To initialise Conda automatically when opening a new terminal:

```
~/miniforge3/bin/conda init bash
```

Close and reopen the terminal.

You can now activate OpenFOAM with:

```
conda activate cfd
```

To deactivate the environment:

```
conda deactivate
```

## **Step 6 — Prepare Your Working Directory**

Once OpenFOAM is installed and activated, create a directory for your CFD simulations:

```
mkdir -p ~/OpenFOAM/run
cd ~/OpenFOAM/run
```

This directory will contain your workshop exercises and simulation cases.

## **Summary**

| Task                          | Command              |
| ----------------------------- | -------------------- |
| Activate OpenFOAM             | `conda activate cfd` |
| Check version                 | `foamVersion`        |
| Generate a structured mesh    | `blockMesh`          |
| Check mesh quality            | `checkMesh`          |
| Run a laminar flow simulation | `icoFoam`            |
| Run a steady-state simulation | `simpleFoam`         |
| Deactivate OpenFOAM           | `conda deactivate`   |

**Important notes:**

- This installation does not require `sudo`.
- All files are installed within the user's home directory.
- OpenFOAM v2412 is used instead of v2606 because it is available through the specified Conda package.
- The instructions assume an x86-64 Linux system.
- ParaView may require a separate installation.

**Official resources:**

- [Miniforge — GitHub](https://github.com/conda-forge/miniforge?utm_source=chatgpt.com)
- [OpenFOAM — OpenCFD](https://www.openfoam.com/?utm_source=chatgpt.com)
- [Conda-forge — OpenFOAM package](https://anaconda.org/conda-forge/openfoam?utm_source=chatgpt.com)
