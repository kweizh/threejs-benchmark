Three.js is transitioning to a modern node-based shader system (TSL) compatible with both WebGPU and WebGL. You need to initialize a `WebGPURenderer` and create a custom node material using `MeshStandardNodeMaterial`. Using TSL functions (`Fn`, `texture`, `color`), create a custom color node that multiplies a provided diffuse texture by a solid red color (`0xff0000`), and assign this to the material's `colorNode`.

**Constraints:**
- You MUST import renderer and TSL utilities strictly from `three/webgpu` and `three/tsl`.
- Do NOT use the legacy WebGL `ShaderMaterial` or write raw GLSL code.