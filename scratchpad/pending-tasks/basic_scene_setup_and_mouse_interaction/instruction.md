Three.js requires a fundamental scene graph, camera, and renderer to display 3D graphics. You need to create a `scene.js` file that initializes a WebGL scene containing a rotating green `BoxGeometry` and a `PointLight`. Additionally, implement a `mousemove` event listener that maps the 2D screen coordinates to 3D space to make the `PointLight` follow the user's cursor.

**Constraints:**
- You MUST export an `initScene(canvasElement)` function.
- Implement a standard animation loop using `requestAnimationFrame`.
- Do NOT use React or `@react-three/fiber`; use vanilla Three.js.