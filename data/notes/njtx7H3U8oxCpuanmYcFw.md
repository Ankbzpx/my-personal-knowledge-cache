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

### Subdivision
```
if bpy.ops.object.mode_set.poll():
    bpy.ops.object.mode_set(mode="OBJECT")
    bpy.ops.object.subdivision_set()
    bpy.ops.object.modifier_apply(modifier="Subdivision")
```

### Triangulate
```
if bpy.ops.object.mode_set.poll():
    bpy.ops.object.mode_set(mode="OBJECT")
    bpy.ops.object.modifier_add(type="TRIANGULATE")
    bpy.ops.object.modifier_apply(modifier="TRIANGULATE")
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

## Bezier curve
```
def create_bezier_curve_object(name, bpts, scale=1.0):
    curve_data = bpy.data.curves.new(name, 'CURVE')
    bevel_obj = bpy.data.objects.new(name, curve_data)
    bpy.context.collection.objects.link(bevel_obj)

    num_of_bpts = int(len(bpts) / 3)
    polyline = curve_data.splines.new('BEZIER')

    # bezier spline is initialized with one bezier_point
    polyline.bezier_points.add(num_of_bpts - 1)
    to_pt3 = lambda pt2: [scale * pt2['x'], -scale * pt2['y'], 0]
    for i in range(num_of_bpts):
        polyline.bezier_points[i].co = to_pt3(bpts[3 * i])
        polyline.bezier_points[i].handle_left_type = 'ALIGNED'
        polyline.bezier_points[i].handle_left = to_pt3(bpts[3 * i + 1])
        polyline.bezier_points[i].handle_right_type = 'ALIGNED'
        polyline.bezier_points[i].handle_right = to_pt3(bpts[3 * i + 2])
```

## Edge loop bevel
```
obj.select_set(True)
bpy.ops.object.convert(target='CURVE')
bpy.data.curves[name].bevel_mode = 'OBJECT'
bpy.data.curves[name].bevel_object = bpy.data.objects[BEVEL_OBJECT_NAME]
bpy.ops.object.convert(target='MESH')
obj.select_set(False)
```
