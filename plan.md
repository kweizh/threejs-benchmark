# Evaluation Dataset Research: Three.js

### 1. Library Overview

*   **Description**: Three.js is the industry-standard JavaScript library for creating and displaying animated 3D computer graphics in a web browser using WebGL and the emerging WebGPU standard. It abstracts the complexities of raw shader programming and GPU management into a high-level, object-oriented API.
*   **Ecosystem Role**: It serves as the foundational layer for almost all 3D web applications, from data visualizations and product configurators to browser-based games and creative portfolios. It is often used alongside frameworks like React (via `@react-three/fiber`), Vue (`tresjs`), and physics engines like `cannon-es` or `rapier`.
*   **Project Setup**:
    1.  **NPM/Vite (Recommended)**:
        ```bash
        npm install three
        npm install --save-dev vite
        ```
    2.  **Importing**:
        ```javascript
        import * as THREE from 'three';
        // Addons must be imported from the 'addons' path
        import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
        ```
    3.  **Import Maps (CDN approach)**: Modern Three.js encourages using Import Maps to manage dependencies without a bundler.
        ```html
        <script type="importmap">
        {
          "imports": {
            "three": "https://cdn.jsdelivr.net/npm/three@0.170.0/build/three.module.js",
            "three/addons/": "https://cdn.jsdelivr.net/npm/three@0.170.0/examples/jsm/"
          }
        }
        </script>
        ```

### 2. Core Primitives & APIs

*   **The Scene Graph**: The fundamental container for all objects, lights, and cameras.
    *   [Scene Documentation](https://threejs.org/docs/#api/en/scenes/Scene)
*   **Renderers**: `WebGLRenderer` (standard) and `WebGPURenderer` (next-gen).
    *   [WebGLRenderer Documentation](https://threejs.org/docs/#api/en/renderers/WebGLRenderer)
*   **Cameras**: `PerspectiveCamera` (realistic) and `OrthographicCamera` (isometric).
    *   [PerspectiveCamera Documentation](https://threejs.org/docs/#api/en/cameras/PerspectiveCamera)
*   **Meshes, Geometries, and Materials**: The "what", "shape", and "look" of 3D objects.
    *   [Mesh Documentation](https://threejs.org/docs/#api/en/objects/Mesh)

#### Code Snippet: Basic WebGL Scene
```javascript
import * as THREE from 'three';

// 1. Scene Setup
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// 2. Add an Object
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

// 3. Add Light
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(5, 5, 5);
scene.add(light);
scene.add(new THREE.AmbientLight(0x404040));

camera.position.z = 5;

// 4. Animation Loop
function animate() {
  requestAnimationFrame(animate);
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();

// 5. Responsive Resizing
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
```

#### Code Snippet: WebGPU & TSL (Three.js Shading Language)
Three.js is transitioning to a node-based shader system (TSL) that works across both WebGL and WebGPU.
```javascript
import * as THREE from 'three/webgpu';
import { texture, uv, color, Fn } from 'three/tsl';

const renderer = new THREE.WebGPURenderer();
await renderer.init();

// TSL Custom Material
const material = new THREE.MeshStandardNodeMaterial();
const customColor = Fn(() => {
  return texture(myTexture).mul(color(0xff0000));
});
material.colorNode = customColor();
```

### 3. Real-World Use Cases & Templates

*   **GLTF Model Loading**: The standard way to bring 3D assets into the web.
    *   [GLTFLoader Example](https://threejs.org/examples/#webgl_loader_gltf)
*   **Interactive Configurators**: Using `Raycaster` to detect clicks on 3D objects and update materials dynamically.
    *   [Raycaster Documentation](https://threejs.org/docs/#api/en/core/Raycaster)
*   **Post-Processing**: Adding Bloom, Depth of Field, or Glitch effects via `EffectComposer`.
    *   [Post-processing Guide](https://threejs.org/manual/#en/post-processing)
*   **React Integration**: `@react-three/fiber` is the most popular way to use Three.js in modern web apps.
    *   [React Three Fiber Documentation](https://docs.pmnd.rs/react-three-fiber/getting-started/introduction)

### 4. Developer Friction Points

*   **GPU Resource Leaks**: Forgetting to call `.dispose()` on geometries, materials, and textures when removing objects from a scene, leading to memory crashes.
    *   [Discussion on Disposal](https://discourse.threejs.org/t/correctly-disposing-entities-in-three-js/21429)
*   **Z-Fighting**: Visual flickering when two surfaces are at the same depth. Solving this requires understanding logarithmic depth buffers or `polygonOffset`.
    *   [Z-Fighting Solutions (StackOverflow)](https://stackoverflow.com/questions/63721425/three-js-weird-rendering-issues-in-close-planes)
*   **Draw Call Bottlenecks**: Rendering thousands of individual meshes instead of using `InstancedMesh` or `BatchedMesh`, which significantly drops FPS.
    *   [InstancedMesh Documentation](https://threejs.org/docs/#api/en/objects/InstancedMesh)

### 5. Evaluation Ideas

1.  **Basic**: Implement a scene with a rotating cube and a point light that follows the mouse.
2.  **Intermediate**: Load a GLTF model and implement a "Loading..." progress bar using the `onProgress` callback.
3.  **Intermediate**: Create a responsive 3D gallery where clicking an image (3D plane) zooms the camera into it.
4.  **Advanced**: Build a particle system of 20,000 stars that move using a custom TSL vertex shader.
5.  **Advanced**: Implement a "Select & Move" tool using `TransformControls` and `Raycaster`.
6.  **Complex**: Optimize a scene with 5,000 unique spheres by merging geometries or using `BatchedMesh` to maintain 60 FPS.
7.  **Complex**: Create a "Water Mirror" effect using a `Reflector` and custom shader nodes for ripples.

### 6. Sources

1.  [Three.js Official Documentation](https://threejs.org/docs/) - Primary API reference.
2.  [Three.js Manual](https://threejs.org/manual/) - Comprehensive guides on fundamentals.
3.  [Three.js llms.txt](https://threejs.org/docs/llms.txt) - Structured overview for AI context.
4.  [Three.js llms-full.txt](https://threejs.org/docs/llms-full.txt) - Detailed API and TSL reference.
5.  [React Three Fiber Pitfalls](https://r3f.docs.pmnd.rs/advanced/pitfalls) - Common performance and architectural issues.
6.  [Three.js Discourse](https://discourse.threejs.org/) - Community discussions on advanced challenges.