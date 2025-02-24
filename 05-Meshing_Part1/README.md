# Pre-Processing in CFD: Meshing with OpenFOAM (part 1)

Meshing is one of the most crucial steps in Computational Fluid Dynamics (CFD) analysis. In this lesson, we’ll take a deep dive into meshing, its role in the Finite Volume Method (FVM), and how to implement it in OpenFOAM using the `blockMesh` utility.

## The Role of FVM in Meshing

To understand meshing, let’s start with a quick recap of the Finite Volume Method (FVM), the numerical approach used in CFD to solve conservation equations. Imagine you are modeling steady, incompressible flow through a pipe. If you know the inlet mass flow rate, you can determine the outlet flow rate by applying conservation of mass. But what if you want to know what’s happening inside the pipe, not just at its boundaries? That’s where meshing comes into play.

Meshing involves dividing a large domain (e.g., the pipe) into smaller, manageable regions called control volumes or cells. Each cell is where conservation equations are solved. This breakdown helps us model the flow in detail and understand the variations of velocity, pressure, and other parameters across the domain.

In the FVM, data such as velocity and pressure are stored at the center of each cell. However, when applying conservation laws, these quantities are evaluated at the cell faces (the boundaries between cells). To achieve this, interpolation is used between the cell centers to calculate values at the faces. This process requires detailed information from the mesh, such as:

- Cell centroids and face centers.
- The connectivity between cells and faces.
- Boundary and internal face types.

Meshing tools like OpenFOAM’s `blockMesh` and `snappyHexMesh` generate this information, enabling us to discretize the equations.

## Mesh Generation in OpenFOAM

OpenFOAM provides two main options for creating a mesh:

- `blockMesh`: A structured meshing tool where the user specifies vertices, blocks, and cell divisions manually.
- `snappyHexMesh`: An automatic meshing tool for creating unstructured meshes around complex geometries.

Alternatively, you can use third-party meshing software to generate a mesh and then import it into OpenFOAM using the available conversion utilities. In this lesson, we’ll focus on `blockMesh`, as it’s the simplest way to understand meshing fundamentals in OpenFOAM.

## Key Mesh Terminology

Before diving into `blockMesh`, let’s clarify some essential terms:

- **Domain**: The region where fluid flow takes place.
- **Cell**: A control volume into which the domain is divided. For example, a hexahedron cell has six faces.
- **Face**: The boundary of a cell. Faces can be:
  - Boundary faces, which border only one cell.
  - Internal faces, which connect two cells.
- **Edge**: The line connecting two vertices.
- **Vertex**: A corner point of a cell.
- **Cell center**: The geometric center of a cell.
- **Face center**: The geometric center of a face.

## A Practical Example: Cavity Flow

Let’s explore the `blockMeshDict` file through a simple example: cavity flow. This example features one of the simplest mesh topologies in OpenFOAM.

### Step 1: Setting Up the Case

Navigate to the run directory in OpenFOAM.

Copy the cavity flow tutorial using the following command:

```bash
cp -r $FOAM_TUTORIALS/incompressible/icoFoam/cavity .
```

Open the copied case directory. You’ll see subdirectories like `system`, `constant`, and `0`. For now, we’ll focus on the `blockMeshDict` file located in the `system` directory.

### Step 2: Running blockMesh

Open the terminal and navigate to the case directory.

Run the `blockMesh` command:
```bash
blockMesh
```
If you run this command outside the case directory, you’ll encounter errors. Always ensure the current directory contains the necessary subdirectories and files.

Once the mesh is generated, you can visualize it using ParaView by running:

```bash
paraFoam
```
### Step 3: Understanding blockMeshDict

The blockMeshDict file contains all the settings for generating the mesh. Here are the key components:

```bash
/*--------------------------------*- C++ -*----------------------------------*\
| =========                 |                                                 |
| \\      /  F ield         | OpenFOAM: The Open Source CFD Toolbox           |
|  \\    /   O peration     | Version:  v2412                                 |
|   \\  /    A nd           | Website:  www.openfoam.com                      |
|    \\/     M anipulation  |                                                 |
\*---------------------------------------------------------------------------*/

FoamFile
{
    version     2.0;          // Version of the OpenFOAM format. '2.0' is standard for blockMeshDict files.
    format      ascii;        // The format used for the dictionary is 'ascii', which is human-readable.
    class       dictionary;   // The file is of type 'dictionary' which is used for settings.
    object      blockMeshDict; // The object name is 'blockMeshDict', indicating it contains block mesh definitions.
}

// * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * //

scale   0.1;  // Scale factor for the entire mesh. This means the coordinates are scaled by 0.1.
              // So, a 1 meter dimension would be scaled to 0.1 meters.

vertices
(
    // Vertices define the 3D coordinates of the 8 corner points of the block mesh.
    (0 0 0)      // Vertex 0: (x0, y0, z0)
    (1 0 0)      // Vertex 1: (x1, y0, z0)
    (1 1 0)      // Vertex 2: (x1, y1, z0)
    (0 1 0)      // Vertex 3: (x0, y1, z0)
    (0 0 0.1)    // Vertex 4: (x0, y0, z1)
    (1 0 0.1)    // Vertex 5: (x1, y0, z1)
    (1 1 0.1)    // Vertex 6: (x1, y1, z1)
    (0 1 0.1)    // Vertex 7: (x0, y1, z1)
);

blocks
(
    // Blocks define the mesh cells, in this case, a hexahedral block.
    // The block uses the 8 vertices (0-7), and it's divided into 20 cells along the x and y directions, and 1 cell along the z direction.
    hex (0 1 2 3 4 5 6 7) (20 20 1) simpleGrading (1 1 1)
    // hex: a hexahedral block is defined using the indices of the vertices (0, 1, 2, 3, 4, 5, 6, 7)
    // (20 20 1): The block is divided into 20 cells along the x-direction, 20 along the y-direction, and 1 cell along the z-direction.
    // simpleGrading (1 1 1): The grading is uniform, meaning the cell sizes will be the same along all directions (x, y, z).
);

edges
(
    // This section is used to define curved edges. In this case, there are no curved edges, so it remains empty.
);

boundary
(
    // Boundary conditions for the mesh:
    
    movingWall
    {
        type wall;  // This defines a 'wall' boundary condition, where no slip condition is applied for velocity.
        faces
        (
            // Defines the top boundary face of the cavity formed by vertices 3, 7, 6, and 2.
            (3 7 6 2)  // Top wall (moving wall)
        );
    }

    fixedWalls
    {
        type wall;  // A 'wall' boundary condition for fixed walls (no-slip).
        faces
        (
            // Left wall defined by vertices 0, 4, 7, and 3.
            (0 4 7 3)
            // Right wall defined by vertices 2, 6, 5, and 1.
            (2 6 5 1)
            // Bottom wall defined by vertices 1, 5, 4, and 0.
            (1 5 4 0)
        );
    }

    frontAndBack
    {
        type empty;  // 'empty' boundary type means this is a 2D domain where the front and back are not physical boundaries.
        faces
        (
            // Front face defined by vertices 0, 3, 2, and 1.
            (0 3 2 1)
            // Back face defined by vertices 4, 5, 6, and 7.
            (4 5 6 7)
        );
    }
);

// ************************************************************************* //
```

#### Explanation of Each Section in `blockMeshDict`

##### Header Section

The header contains metadata about the file. This information helps OpenFOAM understand the format and version of the dictionary.

- **FoamFile**: Standard for every OpenFOAM dictionary file to define the format, version, and object.
- The **class** is `dictionary`, and the **object** is `blockMeshDict` because this file contains mesh settings for `blockMesh`.

##### Scale Factor

- **scale 0.1**: This line sets the scale factor for the mesh to 0.1. All the coordinates listed in the `vertices` section will be scaled by this factor. This is important if you want to specify your domain in centimeters, millimeters, or any other unit but wish to work in meters for OpenFOAM simulations.

##### Vertices

- The **vertices** section defines the coordinates for the 8 corner points of a 3D mesh block (a hexahedron).
- Each coordinate is specified in the format `(x y z)`.
- These vertices define the corners of the cavity mesh in 3D space.

##### Blocks

The **blocks** section defines how to divide the geometry into smaller cells (hexahedra).

- **hex (0 1 2 3 4 5 6 7)**: This line connects the vertices from the `vertices` section to form a hexahedral block.
- **(20 20 1)**: Specifies the number of cells in the x, y, and z directions. Here, we have 20 cells in both the x and y directions, and only 1 cell along the z direction (because it's a 2D cavity flow).
- **simpleGrading (1 1 1)**: Specifies that the grading (size variation) of the cells is uniform across all directions. This means that the cells will have the same size in x, y, and z.

##### Edges

The **edges** section is used if there are curved edges in the domain. In this case, it is left empty because we are working with a simple rectangular mesh where all edges are straight.

##### Boundary Conditions

The **boundary** section defines the conditions on the domain boundaries:

- **movingWall**: This is a moving wall at the top of the cavity. It is treated as a wall where the flow has a no-slip condition.
- **fixedWalls**: These are the fixed boundary walls (left, right, and bottom) of the cavity. They are also wall boundaries with a no-slip condition.
- **frontAndBack**: These faces are treated as "empty" because the simulation is 2D (the front and back faces do not exist in this 2D domain). They are not physical walls but represent the open boundaries in the z-direction.

  <hr>

## Pro Tips for Using blockMesh
- **3D Meshes Only:** OpenFOAM always uses 3D meshes, even for 2D or 1D simulations. For a 2D case, create one cell in the third dimension and set the boundary type in that direction to empty.
- **Indexing:** OpenFOAM uses zero-based indexing for vertices. The first vertex is numbered 0, the second is 1, and so on.
- **Right-Hand Rule:** Ensure the order of vertices follows the right-hand rule for creating blocks. Place your right hand at the first vertex (0) with your thumb pointing in the z-direction. The rest of your fingers should curl toward the next vertices in order.
- **Boundary Faces:** Write boundary faces in counter-clockwise order when viewed from outside the domain.
