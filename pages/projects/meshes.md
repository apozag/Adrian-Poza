# Mesh Processing

Hey there! During my master course, I took classes about meshes and different operations that can be performed upon it. I found this subject extremely interesting and learned new ways of thinking about meshes. In this page I will talk about some of the projects that I found specially usefull or simply curious.

## Find degenerated triangles

You have probably heard of 3D scans that can capture real world objects or environments and turn them into a 3D mesh. This is great and all, but the techology is not quite perfect and creates imoperfections and triangles that are degenerated. When we say degenerated we mean the area of the triangle is too small compared to the length of its sides. When it comes to rendering, it doesn't really matter if the geometry is a mess as long as the overall thing looks fine since the GPU sucks it all up and doesn't get fussy about weird geometry. Nevertheless, it might be a problem when we want to compute satistics of the mesh or perform any other kind of operation on it. To fix it, we will have to prepare the data first. When we load a mesh, we might get different mesh conectivity information. In this case, we would prefer having information about triangle conectivity, since we want to access the vertices of the same triangle (or face) at the same time without having to find the neighbours every time we access a vertex. Once we have this information, we can interate through the triangles and find out its shape factor, which will let us determinte if it is a degenerated triangle based on a threshold: 

 ```c++
double shapeFactor = minLen / (circumradius * sqrt(3));
 ```

 Where minLen is the shortest side of the triangle and circumradius is the radius of the circle that passes through the three vertices. We will store an array with the resulting value of this operation for every triangle and then change the color of the vertices that correspond to those triangles that dont pass the test so we can later visualize it.

 ![Stats](https://apozag.github.io/Adrian-Poza/images/statistics.PNG)


## Calculate UV coordinates

In this exercise, the goal is to 'sperad' the mesh UVs in a square so we get something like this: 

![Mannequin](https://apozag.github.io/Adrian-Poza/images/mannequin-uv.PNG)
![Mannequin](https://apozag.github.io/Adrian-Poza/images/mannequin-textured.PNG)

For this to work, we need a mesh that has the topology of a disk, so it has a boundary. The first step would be to detect the vertices that are part of this boundary so we can clamp them to the borders of the square (which, of couse, has 1x1 unit size). The rule to find a boundary edge is trying to find the reversed edge. If it does not exist, it's a boudary edge! This sounds extremely slow and it is, but probably not as much as you think. We will use two containers for this algoritm: one for external (boundary) and another for internal edges. Both will be empty initially. We will iterate over the triangles and the break down each edge. For each edge, we search for the reversed edge in the extenal container. If it is not found, we store it in external for now. Otherwise, if it is found, then we know that edge is actually internal, so we remove the reversed one from external and store the current edge in internal. At the end of the algorithm, we will have separated the two kinds correctly. The external vertices will then be ordered so they form a cicle. This way we know how to spread the boundary around the borders of the UV square.

The uv coordinate will be computed using a linear equation system. For that, we need to compute a laplacian matrix. We want to minimize the the sum of the cotangent of the angles at opposite vertices of the two triangles that share the edge formed by vertex i and vertex j, like shown in the following scheme: 

      /\         .  Lij = cot(α) + cot(β) , if vertex i neighbour of j
     /β \        .
    /    \       .  Lii = -Sum Lij , diagonal element is the sum of row
   /      \      .
 vi--------vj    .
   \      /      .
    \    /       .
     \α /        .
      \/         .

