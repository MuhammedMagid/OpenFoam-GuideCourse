# Implementing a CFD Analysis in OpenFOAM

## Introduction

In this lesson, we will walk through the steps of implementing a Computational Fluid Dynamics (CFD) analysis using OpenFOAM. OpenFOAM is a powerful open-source CFD software used for simulating fluid dynamics problems across various fields such as engineering, environmental studies, and aerodynamics. Although OpenFOAM doesn't have a built-in graphical user interface (GUI), its flexibility and command-line interface make it a preferred tool for many CFD professionals and researchers. The analysis process in OpenFOAM involves several key stages, including pre-processing, processing, and post-processing. This lesson will guide you through each of these stages, ensuring that you understand the workflow and key concepts of OpenFOAM.

## CFD Analysis Flowchart

Before we begin, it’s important to visualize the overall CFD process. Below is a flowchart outlining the key stages of a CFD analysis:

![cfdPhases](https://github.com/user-attachments/assets/5ae71cb6-ee3f-41f1-ad79-34531e26151c)


1. **Pre-Processing**
    - Gather information about the fluid flow system.
    - Generate a computational mesh.
    - Select relevant physics.
    - Specify boundary and initial conditions.
  
2. **Processing**
    - Discretize the equations using numerical schemes.
    - Set iterative solver parameters.
    - Check for convergence.

3. **Post-Processing**
    - Analyze and visualize the simulation results.

This flowchart serves as a helpful reference for understanding the stages of any CFD analysis.

## OpenFOAM Basics

Before diving into a hands-on example, let’s go over some key points about OpenFOAM.

- **No GUI**: OpenFOAM doesn’t come with a graphical user interface (GUI) by default. Instead, you interact with the software through the terminal and use a text editor for input file modification.
  
- **Terminal & Text Editor**:
    - **Terminal**: Used to execute OpenFOAM commands.
    - **Text Editor**: Used to modify the input files that define simulation parameters. For instance, you will use a text editor to modify `blockMeshDict` (for mesh generation) or `controlDict` (for solver settings).

## Case Directory Structure

OpenFOAM organizes the simulation files into a case directory structure, with three main subdirectories:

1. **system**:
    - **controlDict**: Controls simulation settings (e.g., time steps, output frequency).
    - **fvSchemes**: Defines numerical schemes (e.g., discretization methods).
    - **fvSolution**: Contains solver settings (e.g., relaxation factors, solver control).

2. **constant**:
    - Defines physical properties (e.g., viscosity, density).
    - Turbulence models (if applicable).
    - Contains mesh

3. **0 Directory**:
    - Defines initial and boundary conditions for the simulation (e.g., initial pressure and velocity fields).

These subdirectories allow for organized management of the files and parameters for each simulation.

## Practical Example: Step-by-Step CFD Analysis

Now, let's break down the process of setting up and running a simulation in OpenFOAM, using a practical example.

### 1. Setting Up the Case

- **Navigate to the Run Directory**:
    First, you need to navigate to the directory where you want to set up the simulation. This is where you will store your case files.

- **Copy a Tutorial Case**:
    OpenFOAM comes with several built-in tutorial cases. A great starting point is to copy one of these tutorials into a new directory for your case.
    ```bash
    cp -r $FOAM_TUTORIALS/incompressible/simpleFoam/pitzDaily .
    cd pitzDaily
    ```

- **Observe the Subdirectories**:
    The case directory will contain the familiar structure with the `system`, `constant`, and `0` directories. Each of these directories contains relevant files for the simulation.

### 2. Creating the Mesh

- **Generate the Mesh**:
    In the `system` directory, you will find a file called `blockMeshDict`. This file defines the geometry and topology of your computational mesh. You can modify this file to fit your problem’s geometry.

    Once the `blockMeshDict` is ready, run the following command in the terminal to generate the mesh:
    ```bash
    blockMesh
    ```

- **Check the Mesh**:
    After the mesh is generated, it’s essential to verify it to ensure there are no errors. You can check the mesh quality using OpenFOAM’s `checkMesh` command:
    ```bash
    checkMesh
    ```

### 3. Defining Initial & Boundary Conditions

- **Navigate to the `0` Directory**:
    The `0` directory contains the initial and boundary conditions for the simulation. Key fields such as pressure (`p`) and velocity (`U`) are defined in this directory.

- **Modify Fields Using a Text Editor**:
    Open the field files in a text editor (such as `vim` or `nano`) to set the initial values and boundary conditions:
    ```bash
    vim 0/U
    vim 0/p
    ```

- **Boundary Conditions**:
    Ensure the boundary conditions are correctly defined in the `0` directory. For example, if you are simulating flow through a pipe, you might define the inlet and outlet conditions here. The boundary names in the `0` files should match the names you defined in the `blockMeshDict`.

### 4. Setting Physical Properties

- **Constant Directory**:
    The `constant` directory holds the physical properties of the fluid, such as density and viscosity. You can also specify any turbulence models here if applicable.

    - **`transportProperties`**: Defines the fluid’s physical properties (e.g., viscosity, density).
    - **`turbulenceProperties`**: If your simulation involves turbulence, you would specify the turbulence model here.

### 5. Solver Settings

- **Solver**:
    OpenFOAM offers the `simpleFoam` solver. This solver is Steady-state solver for incompressible, turbulent flows.

    - You can review and modify the solver settings in the `system/controlDict` file. This file defines the time control parameters, like start and end time, and output settings.
    
    - Other key files include:
        - **`fvSchemes`**: Contains the numerical schemes for discretization.
        - **`fvSolution`**: Defines solver parameters like residual control and relaxation factors.

### 6. Running the Simulation

Once all the setup is complete, it's time to run the simulation:

```bash
simpleFoam
```
This will start the solver and begin the iterative process of solving the governing fluid equations. During the simulation, OpenFOAM will display residuals and other diagnostics in the terminal.

### 7. Post-Processing: Visualizing the Results
After the simulation completes, it’s time to analyze the results. OpenFOAM generates output data in the form of time-step directories.

Post-Processing Tools: You can use OpenFOAM's built-in tools like paraFoam to visualize the simulation results in ParaView, a popular open-source visualization tool.

To launch ParaView with your case data, simply run:

```bash
paraFoam
```
In ParaView, you can plot various quantities like pressure, velocity, or turbulence variables, and gain insights into the flow behavior.

<hr>

By following these steps, you should now be able to perform a CFD analysis in OpenFOAM. This includes creating the mesh, defining initial and boundary conditions, setting up solver parameters, running the simulation, and visualizing the results. While OpenFOAM may not offer a GUI, its flexibility and powerful command-line interface make it a versatile tool for a CFD simulations.
