## Introduction

This is a Houdini implementation of Conformal Mapping [1]. The point is to apply a UV texture map onto a triangle mesh (with boundary) while preserving angles between texture details and avoiding local stretching.

## Visualizations

### Without Conformal Mapping

Textures are distorted orthographically. 

![alt text](/images/bettle.png)

### With Conformal Mapping

![alt text](/images/conformed_bettle.png)

## Implementation Details

To obtain conformal coordinates, we find a mapping from the UV space to the complex space ($z = u + iv$) that minimizes the discretized conformal energy:

$$ C[z] = z^HCz $$

where 

$$C = \frac{1}{2}L - A$$

$L$ is the graph Laplacian [2] and $A$ is the signed area of the triangle mesh. The optimization problem is:

$$min \left(\frac{z^HCz}{z^H ★_0 z}\right)$$

where $★_0$ is the mass matrix that encodes per-vertex area weights. Here, we minimize the Rayleigh quotient (instead of just the numerator) to avoid trivial solution $z^HCz = 0$. The solution is given by the eigenvalue problem:

$$Cz = \lambda ★_0 z$$

The desired $z$ is the eigenvector corresponding to the smallest eigenvalue $\lambda$. Finally, we assign $u$ and $v$ to the real and imaginary parts of $z$. This completes the conformal mapping.

## References

[1] https://en.wikipedia.org/wiki/Conformal_map
[2] https://en.wikipedia.org/wiki/Laplacian_matrix


