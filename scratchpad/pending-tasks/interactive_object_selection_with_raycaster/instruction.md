Interactive web experiences often require clicking and manipulating 3D objects. You need to implement object picking using `Raycaster` in a scene containing multiple overlapping meshes. When a user clicks on a mesh, detect the intersection, update the intersected material's color to indicate selection, and attach a `TransformControls` instance to allow the user to drag and move the selected object.

**Constraints:**
- Only apply the selection and attach controls to objects where the `userData.isSelectable` property is `true`.
- Do NOT select objects hidden behind other objects (only the closest intersected object should be selected).