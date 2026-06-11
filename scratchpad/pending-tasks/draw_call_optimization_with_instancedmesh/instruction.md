Rendering thousands of individual meshes creates severe draw call bottlenecks that drop the frame rate. You are given an unoptimized script that loops 5,000 times to create 5,000 unique `Mesh` instances of spheres. You need to refactor this script to use a single `InstancedMesh` to render all 5,000 spheres in a single draw call while maintaining their original individual 3D positions.

**Constraints:**
- The visual output (positions, scales, and base material) must remain exactly the same as the unoptimized version.
- Use `Object3D` as a dummy to compute and set the local transformation matrix for each instance via `setMatrixAt`.