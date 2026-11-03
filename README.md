# Texture Painter (Godot 4.7)

Paint a texture layer on any `MeshInstance3D`, right in the editor's 3D viewport,
through the mesh's UVs. Even a very low-poly mesh gets sharp detail. Your materials
and textures aren't changed: the paint is a separate layer on top.

Split out of [godot-vertex-painter](https://github.com/Juljus777/godot-vertex-painter)'s
**Paint on: Texture** mode.

## Use

1. Enable the plugin (Project Settings → Plugins → TexturePainter).
2. Select a mesh, tick **Enable Texture Painting** in the TexturePainter dock.
3. Paint in the 3D viewport.

| Input | Does |
|---|---|
| Drag | paint |
| Shift+drag | erase |
| Alt+click / drag | pick the color under the cursor (keeps the opacity) |
| Ctrl+click | select the mesh under the cursor |

- **Brush**: the shape of a dab.
  - *Soft Edge*: full strength with a soft rim (default).
  - *Hard*: full strength up to the edge.
  - *Linear Fade*: fades evenly from the center to the edge.
  - *Smooth Fade*: fades from the center to the edge on an S-curve, like an airbrush.
  - *Spray*: scattered speckles, denser toward the center. Each stroke gets a new pattern, so going over an area again fills it in.
- **Brush Radius** is in meters. A translucent sphere shows the brush under the cursor.
  Strokes stay round on rotated, scaled or stretched meshes and continue across UV seams.
- The color picker's **A** slider is the opacity. One stroke never paints more than
  that; strokes on top of each other build up.
- **Resolution** is the image size, picked when a mesh is first painted (default 1024).
- Each stroke is one Ctrl+Z.
- Saving the scene writes one PNG per mesh to a `<scene name>_paint` folder next to
  the scene. Keep those files with the scene; you can edit them in other image editors.

## Limitations

- The mesh needs clean UVs (UV1): inside 0–1, no overlapping or tiling. A mesh
  without UVs can't be painted.
- The layer is the node's **Material Overlay**, so a node that already has one can't be painted.
- Only the first surface's UVs are painted, but the layer is drawn over every surface.
- The brush is a sphere: a large brush also paints surfaces just behind the one
  under the cursor, like the other side of a thin wall.
- Duplicating a painted node shares its layer until you make the Material Overlay unique.
- Other objects don't block the brush: it paints the selected mesh under the cursor.
- For a mesh inside an instanced scene (e.g. a `.glb`), turn on **Editable Children** first.
- Very large brushes at 4096 are slow; normal sizes at 1024 take about a millisecond per dab.

## Tests

```
GODOT=/path/to/godot-4.7 tests/check_editor.sh
```

Runs the headless brush tests (`tests/texture_paint_probe.gd`) and drives the
plugin in a headless editor (`tests/editor_probe/`).
