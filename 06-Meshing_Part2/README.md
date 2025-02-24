# Pre-Processing in CFD: Meshing with OpenFOAM (part 2)

Welcome to another installment of the OpenFOAM Guide Course! In this lesson, we’ll take a deep dive into meshing with blockMesh by walking through a classic fluid flow example: **Poiseuille Flow**.

Through this example, you’ll learn:
- How to define the domain of your project.
- How to systematically move from a simple schematic to a computational mesh.
- How to set up boundary conditions and choose a solver in OpenFOAM.

Let’s get started!

---

### **Understanding the Problem: Poiseuille Flow**

Poiseuille flow is a simple yet essential fluid dynamics case: fluid flow between two parallel plates under a pressure gradient. It’s widely used in CFD studies because it has a well-known analytical solution, making it easy to validate simulation results.

The first step in any CFD analysis is to identify the **fluid domain** and any internal boundaries. In this case, the domain consists of a fluid flow between two plates with high pressure at the inlet and lower pressure at the outlet. We assume:
- **Steady, incompressible, and laminar 2D flow.**
- **Symmetry** in the flow, allowing us to simulate only half of the domain to save computational time.

---

### **Defining Boundary Conditions**

To properly set up the simulation, we define the boundary conditions:
- **Inlet**: Fixed velocity.
- **Outlet**: Atmospheric pressure.
- **Walls (Bottom)**: No-slip condition (velocity = 0).
- **Symmetry Plane**: Since we model only half of the domain, we set the upper wall as a symmetry plane.

Additionally, to ensure a fully developed flow profile, the **domain length must be greater than the entrance length**.

---

### **Creating the Mesh with blockMesh**

Now, let’s see how blockMesh defines the domain. OpenFOAM operates in **3D space**, so even though our problem is 2D, we must define at least one cell in the **z-direction**.

#### **Defining Vertices**
We choose the **origin (0,0,0)** as our reference point and define subsequent vertices in Cartesian coordinates:
- Move **30 mm in the x-direction** to define the length.
- Move **1 mm in the y-direction** to define the height.
- The front face is identical but offset in the z-direction.

📝 **Tip:** The order of vertices doesn’t matter, but their index numbers are assigned sequentially. Be careful when defining blocks and boundary faces!

#### **Choosing the Solver**
For our incompressible, steady-state problem, we use **simpleFoam**, a steady-state solver. Alternatively, we could use **icoFoam**, a transient solver, which would yield the same results.

---

### **Setting Up the Simulation Case**

Instead of manually creating all input files, we take a **shortcut**: modifying an existing tutorial case.

#### **Steps:**
1. Navigate to OpenFOAM’s **tutorials directory**.
2. Find a case similar to ours (e.g., pitzDaily).
3. Copy it and modify its input files.

#### **Modifying blockMeshDict**
- Delete unnecessary vertices, variables, and blocks.
- Define only **one block** with appropriate **cell counts and grading**.
- Adjust boundary definitions and ensure correct face orientation.

Once the mesh is ready, we **run blockMesh**. If errors occur, reading the terminal output helps debug them.

📝 **Tip:** Learning to interpret Linux command outputs is crucial, as OpenFOAM lacks a GUI.

#### **Checking the Mesh**
- Use **checkMesh** to verify mesh quality.
- Visualize the mesh in **ParaView** by running `paraFoam`.

---

### **Understanding Mesh Data Files**
OpenFOAM stores mesh data in structured text files:
- **boundary**: Contains a list of all faces forming the boundary patches. This file is always written in ASCII format.
- **faces**: Defines all the faces of the mesh. Each face is described by the points that form it.
- **neighbour**: Lists the neighboring cells for each face.
- **owner**: Contains a list of the owning cells for each face.
- **points**: Provides the coordinates of all the points in the mesh.

While you rarely need to modify these files, understanding their structure is beneficial.

---

### **Modifying Solver Input Files**
We now configure the remaining input files:

#### **1. Transport Properties**
- Select a suitable viscosity model.
- Set fluid viscosity values.

#### **2. Turbulence Properties**
- Since our case is laminar, we set `simulationType` to **laminar**.

#### **3. Initial & Boundary Conditions**
- **p (Pressure)**: Define inlet, outlet, and wall conditions.
- **U (Velocity)**: Specify the inlet velocity and enforce no-slip conditions on walls.
- **fvSchemes & fvSolution**: No major modifications needed for this case.

Finally, we run **simpleFoam** and check for errors. If all goes well, we proceed to post-processing!

---

### **Post-Processing and Validation**

To validate the results, you plot:
- **Velocity Profile**: Expected parabolic distribution.
- **Pressure Drop**: Should match the analytical solution.

Using **ParaView**, you can visualize and compare your simulation results against theoretical predictions.

---

### **Final Thoughts**

By following these steps, you’ve successfully set up, meshed, and solved Poiseuille flow using OpenFOAM. This approach can be applied to other fluid flow problems, helping you develop a deeper understanding of meshing and solver configurations.

If you want to **practice further**, try modifying the mesh density, adjusting boundary conditions, or using a transient solver to observe unsteady effects.


---

