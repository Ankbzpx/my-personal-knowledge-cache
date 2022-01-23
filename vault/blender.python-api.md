---
id: njtx7H3U8oxCpuanmYcFw
title: Python API
desc: ''
updated: 1642932882525
created: 1642932434975
---
## Mesh ops
```
if bpy.ops.object.mode_set.poll():
    bpy.ops.object.mode_set(mode="EDIT")
    bpy.ops.mesh.edge_face_add()
    bpy.ops.mesh.faces_shade_smooth()
    bpy.ops.object.mode_set(mode="OBJECT")
```

## Join
```
if bpy.ops.object.mode_set.poll():
    bpy.ops.object.mode_set(mode="OBJECT")
    
    for obj in objs:    
        obj.select_set(True)
    
    # at least one of them needs to be active
    bpy.context.view_layer.objects.active = objs[0]

    bpy.ops.object.join()
    obj = bpy.context.selected_objects[0]
    obj.select_set(False)
```

## Modifier
```
if bpy.ops.object.mode_set.poll():
    bpy.ops.object.mode_set(mode="OBJECT")
    bpy.ops.object.subdivision_set()
    bpy.ops.object.modifier_apply(modifier="Subdivision")
```
## UV
```
uvlayer = mesh.uv_layers.new()
mesh.uv_layers.active = uvlayer
for face in mesh.polygons:
    for vert_idx, loop_idx in zip(face.vertices, face.loop_indices):
        uv = np.array(mesh.vertices[vert_idx].co[:2]) / 2.0 + 0.5
        mesh.uv_layers.active.data[loop_idx].uv = uv
```

## Material
```
# load image
bpy.ops.image.open(filepath=image_path)
# create bsdf material
mat = bpy.data.materials.new(image_path)
mat.use_nodes = True
for node in mat.node_tree.nodes:
    if node.type == "BSDF_PRINCIPLED":
        node.select = True
        # create image texture
        image_texture_node = mat.node_tree.nodes.new("ShaderNodeTexImage")
        image_texture_node.image = bpy.data.images[image_path]
        # assign image texture to bsdf base color
        mat.node_tree.links.new(node.inputs['Base Color'], image_texture_node.outputs['Color'])
mesh.materials.append(mat)
```