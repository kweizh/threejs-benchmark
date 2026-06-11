Loading external 3D models efficiently is a standard requirement for web applications. You need to write a script using `GLTFLoader` to load a 3D model from `./assets/model.gltf` and add it to an existing scene. Implement the loader's `onProgress` callback to calculate the loading percentage and dynamically update the `innerText` of an existing HTML DOM element with the ID `loading-progress`.

**Constraints:**
- The script must correctly import `GLTFLoader` from the `three/addons/` path.
- Handle both the `onLoad` (adding the model to the scene) and `onProgress` callbacks.