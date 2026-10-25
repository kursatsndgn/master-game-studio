---
name: blender-3d-pipeline
description: Automated procedural 3D modeling, UV unwrapping, PBR material creation, LOD generation, collision mesh generation, and multi-format asset export using Blender Python (bpy) in headless or live mode.
---

# Blender 3D Pipeline Skill

The **Blender 3D Pipeline** skill automates 3D content creation workflows using Blender's Python API (`bpy`). It generates clean procedural geometry, automatically unwraps UVs, sets up PBR node networks, builds LOD cascades, and exports assets directly into game engine projects.

---

## 1. Execution Modes

1. **Headless CLI Execution (Zero-GUI / CI/CD)**:
   ```bash
   blender -b -P scripts/generate_asset.py -- --name "prop_chest" --output "exports/prop_chest.gltf"
   ```
2. **Interactive Live Bridge via Blender MCP**:
   Communicates directly with an active Blender instance via TCP/stdio Model Context Protocol commands.

---

## 2. Standard Procedural Generator Template

Below is the production standard Python template for procedural asset generation:

```python
import bpy
import bmesh
import math
import sys
import argparse

def reset_scene():
    """Wipes all default objects, meshes, and materials from the scene."""
    bpy.ops.wm.read_factory_settings(use_empty=True)

def create_procedural_chest(width=1.0, depth=0.6, height=0.7):
    """Creates a base chest mesh with beveled edges and UV coordinates."""
    # Create base mesh
    mesh = bpy.data.meshes.new("Chest_Base")
    obj = bpy.data.objects.new("SM_Chest_Base", mesh)
    bpy.context.collection.objects.link(obj)
    bpy.context.view_layer.objects.active = obj
    obj.select_set(True)

    # Build geometry with BMesh
    bm = bmesh.new()
    bmesh.ops.create_cube(bm, size=1.0)
    # Scale to dimensions
    bmesh.ops.scale(bm, vec=(width, depth, height), verts=bm.verts)
    bm.to_mesh(mesh)
    bm.free()

    # Add Bevel Modifier for clean gaming edges
    bevel = obj.modifiers.new(name="Bevel", type='BEVEL')
    bevel.width = 0.02
    bevel.segments = 2
    bevel.limit_method = 'ANGLE'
    bpy.ops.object.modifier_apply(modifier="Bevel")

    # Smart UV Project
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.uv.smart_project(angle_limit=math.radians(66), island_margin=0.02)
    bpy.ops.object.mode_set(mode='OBJECT')

    return obj

def build_pbr_material(obj, mat_name="M_Chest_Wood"):
    """Constructs a standard PBR Principled BSDF node setup."""
    mat = bpy.data.materials.new(name=mat_name)
    mat.use_nodes = True
    nodes = mat.node_tree.nodes
    bsdf = nodes.get("Principled BSDF")
    
    # Base material attributes
    bsdf.inputs['Base Color'].default_value = (0.35, 0.20, 0.10, 1.0) # Wood brown
    bsdf.inputs['Roughness'].default_value = 0.75
    bsdf.inputs['Metallic'].default_value = 0.05
    
    obj.data.materials.append(mat)
    return mat

def create_lods_and_export(obj, export_filepath, lod_ratios=[1.0, 0.4, 0.15]):
    """Generates LOD0, LOD1, LOD2 and exports as a unified glTF/GLB."""
    base_name = obj.name
    lod_objects = []

    for i, ratio in enumerate(lod_ratios):
        lod_obj = obj.copy()
        lod_obj.data = obj.data.copy()
        lod_obj.name = f"{base_name}_LOD{i}"
        bpy.context.collection.objects.link(lod_obj)
        
        if ratio < 1.0:
            decimate = lod_obj.modifiers.new(name="Decimate", type='DECIMATE')
            decimate.ratio = ratio
            bpy.context.view_layer.objects.active = lod_obj
            bpy.ops.object.modifier_apply(modifier="Decimate")
        
        lod_objects.append(lod_obj)

    # Export glTF 2.0 with embedded buffers
    bpy.ops.export_scene.gltf(
        filepath=export_filepath,
        export_format='GLB',
        use_selection=False,
        export_apply=True,
        export_yup=True
    )
    print(f"[SUCCESS] Exported asset to {export_filepath}")

if __name__ == "__main__":
    reset_scene()
    chest = create_procedural_chest()
    build_pbr_material(chest)
    create_lods_and_export(chest, "SM_Chest.glb")
```

---

## 3. Collision Mesh Convention

When exporting assets for game engines, automated collision meshes are generated using standard DCC naming prefixes:
- **`UCX_AssetName_01`**: Convex Hull collision mesh (recognized by Unreal Engine, Unity, and Godot importers).
- **`UBX_AssetName_01`**: Box collision primitive.
- **`USP_AssetName_01`**: Sphere collision primitive.

---

## 4. Asset Export Matrix

| Target Engine | Preferred Format | Texture Embedding | Coordinate Orientation |
| :--- | :--- | :--- | :--- |
| **Godot 4** | `.glb` / `.gltf` | Embedded or relative PBR | Y-Up, -Z Forward |
| **Unity** | `.glb` or `.fbx` | External `.png` / `.tga` | Y-Up, Z Forward |
| **Unreal Engine 5** | `.fbx` or `.usd` | Texture Maps in `/Textures/` | Z-Up, X Forward (Nanite enabled) |
