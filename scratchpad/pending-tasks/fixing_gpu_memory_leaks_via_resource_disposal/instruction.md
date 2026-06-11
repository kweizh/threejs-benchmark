A major friction point in Three.js is GPU resource leakage caused by removing objects from the scene without freeing their associated data. You are provided with a buggy `dynamicGallery.js` file that continuously removes old meshes and adds new ones, causing browser crashes over time. You need to fix the memory leak by implementing a cleanup function that correctly calls `.dispose()` on all geometries, materials, and textures associated with the removed meshes.

**Constraints:**
- Do NOT change the timing or structure of the mesh addition/removal logic.
- Ensure that textures applied to materials are also disposed of properly.