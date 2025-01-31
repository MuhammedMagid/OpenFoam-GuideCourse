# Setting Up the OpenFOAM Environment

## Introduction

Installing OpenFOAM is straightforward, much like any other software. However, many users get confused when choosing between different OpenFOAM forks or releases. This guide will clarify those differences and walk you through the installation process.

## Understanding OpenFOAM Forks

OpenFOAM exists in three main forks:

- **OpenCFD** – Developed by ESI Group.
- **Foundation** – The official standard release.
- **OpenFOAM-Extended** – A community-driven fork.

The differences between these versions are mostly relevant to developers, so as a beginner, you can choose any of them. For this guide, we will use the OpenCFD version developed by ESI Group.

## Downloading OpenFOAM

To install OpenFOAM, visit the official website:

- [OpenCFD version](https://www.openfoam.com)
- [Foundation version](https://www.openfoam.org)

You can switch between versions if needed, as the transition is smooth.

## Exploring the OpenFOAM Website

On the OpenFOAM website, you will find information about:

- The history and development of OpenFOAM.
- Release cycles (two per year: June and December).
- Version details and previous releases.

## Installing OpenFOAM

OpenFOAM is available for multiple operating systems. Choose your OS and follow the installation steps:

- For **Linux (Ubuntu)**, copy and run the provided commands in the terminal.

```bash
# Add the repository
curl https://dl.openfoam.com/add-debian-repo.sh | sudo bash

# Update the repository information
sudo apt-get update

# Install preferred package. Eg,
sudo apt-get install openfoam2412-default
```

The package naming convention follows this format:

- **openfoam** – OpenCFD trademark.
- **2412** – Version naming (Year-Month).
- **default** – Includes all essential components.

## Activating OpenFOAM

After installation, running OpenFOAM commands like `blockMesh` may result in an error. This is because the OpenFOAM environment needs to be activated:

1. Start an OpenFOAM session with: `openfoam2412`
2. Manually source OpenFOAM in your bashrc file.

Once sourced, OpenFOAM environment variables are added, allowing direct execution of commands.

## Installing Supplemental Tools

OpenFOAM does not include visualization tools, so you may need additional software:

- **ParaView** – For graphical post-processing.
- **Gnuplot** and **PyFoam** – Optional tools for analysis.

## Final Check

To ensure OpenFOAM is properly installed and configured, run:

```bash
blockMesh
```
or
```bash
foam
