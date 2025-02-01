# OpenFOAM Directory Structure | Overview

## Introduction

Hey there, OpenFOAM users!

Whether you're just starting out with OpenFOAM or you're already deep into it, one thing that can really make your life easier is understanding the OpenFOAM directory structure. Knowing where things are located and how everything is organized will help you work more efficiently and navigate through OpenFOAM with ease.

So, let’s dive into the details of how OpenFOAM is laid out!

---

## Navigating the OpenFOAM Directory Structure

When you first install OpenFOAM, you'll notice a bunch of subdirectories inside the main OpenFOAM directory. These directories serve different purposes and knowing what each one is for is key to mastering OpenFOAM.

### Accessing the OpenFOAM Directory

There are a couple of ways to get to the OpenFOAM directory from the terminal:

- **Using the `cd` command**: Simply type `cd` followed by the path variable pointing to OpenFOAM.
- **Using the `foam` alias**: This handy shortcut takes you straight to the OpenFOAM directory.

In this guide, we'll go over both methods and introduce some useful variables that make your life easier, whether you're navigating or performing other tasks.

By the way, you can use a file manager (like Nautilus on Linux, Finder on macOS, or File Explorer on Windows) to navigate the OpenFOAM directory, but it's typically less efficient than using the terminal, especially when working with OpenFOAM's complex directory structure.

---

## Breakdown of OpenFOAM Directories

Here’s a closer look at the key directories within the OpenFOAM installation.

### 1. **`applications/`**

This is where you'll find pre-built OpenFOAM applications, organized into the following categories:

- **Solvers**: These are algorithms designed to solve specific CFD problems. They are grouped by problem type (e.g., incompressible, compressible, multiphase flows).
- **Utilities**: These tools are used for tasks like pre- and post-processing, mesh generation, e.g. data manipulation, and visualization.

#### Navigating to `applications/`
- You can use the OpenFOAM variable followed by the subdirectory name.
- Or, use the alias `app` for quick access.


### 2. **`bin/`**

This directory contains various shell scripts that help automate tasks within OpenFOAM. 

- **Examples**: 
  - `paraFoam`: Opens OpenFOAM results in ParaView.
  - `foamCleanTutorial`: Cleans up generated files in tutorial cases.
  - `foamNewApp`: Creates a new application needed files.

### 3. **`etc/`**

Here, you'll find configuration files and settings that control environment variables, paths, and preferences for OpenFOAM.

### 4. **`platforms/`**

This directory holds the binaries for OpenFOAM, with the following subdirectories:
- **`bin/`**: Contains script and application binaries.
- **`lib/`**: Contains compiled library files.

### 5. **`src/`**

This is the heart of OpenFOAM—where the source code of all libraries lives. Here are some key libraries you'll encounter:

- **`finiteVolume/`**: Contains classes for finite volume discretization, including mesh handling and operators (like divergence, gradient, and Laplacian).
- **`transportModels/`**: Contains models for fluid transport properties.
- **`turbulenceModels/`**: Libraries for turbulence modeling.

### 6. **`doc/`**

The `doc/` directory contains documentation you need to get a deeper understanding of how things work.

### 7. **`tutorials/`**

The `tutorials/` directory is full of example cases designed to help you learn OpenFOAM. These cases cover a wide range of simulation scenarios, and we’ll dive into these in later videos.

### 8. **`wmake/`**

This directory contains the build scripts needed to compile OpenFOAM components. It's based on `make`, but with customizations to understand the OpenFOAM structure.

---

## The User Directory

In addition to the OpenFOAM installation directory, users typically work within their **user directory**.

### Key Components in Your User Directory:
- **Run directory**: This is where you set up your simulation cases.
- **Applications directory**: This is where you can modify or develop custom solvers or utilities.

#### Important Note:
Changes should *never* be made directly inside the OpenFOAM installation directory. 

---

## Conclusion

That’s a quick look at the OpenFOAM directory structure!

As a new user of OpenFOAM, understanding how everything is organized is super helpful. Each directory has a specific job, and knowing where to find things will make your work easier and faster.

Take some time to explore the directories and get comfortable with the layout. The more familiar you are, the smoother your OpenFOAM projects will go.

Thanks for reading!

---



