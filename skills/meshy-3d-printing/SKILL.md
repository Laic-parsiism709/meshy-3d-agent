---
name: meshy-3d-printing
description: 3D print models generated with Meshy AI. Handles printability analysis, slicer integration, multi-color printing guidance, and print-optimized download workflows. Use when the user mentions 3D printing, slicing, Bambu, OrcaSlicer, Prusa, Cura, Creality Print, or wants to print a figurine, miniature, or physical model.
license: MIT
compatibility: Requires Python 3 with requests package. Depends on meshy-3d-generation skill. Works with Claude Code, Cursor, and all Agent Skills compatible tools.
metadata:
  author: meshy-dev
  version: "1.0.0"
  homepage: https://github.com/meshy-dev/meshy-3d-agent
allowed-tools: Bash, Read, Write, Glob, Grep
---

# Meshy 3D Printing

Prepare and send Meshy-generated 3D models to a slicer for 3D printing. This skill covers the full print pipeline: generating print-ready models, checking printability, downloading in slicer-compatible OBJ format, and opening in the user's preferred slicer software.

**Prerequisite:** This skill works alongside `meshy-3d-generation`. The generation skill provides the reusable script template (`create_task`, `poll_task`, `download`, `get_project_dir`, etc.) and environment setup. If the model has not been generated yet, use `meshy-3d-generation` first.

---

## Intent Detection

Proactively suggest 3D printing when these keywords appear in the user's request:
- **Direct**: print, 3d print, slicer, slice, bambu, orca, prusa, cura
- **Implied**: figurine, miniature, statue, physical model, desk toy, phone stand

When detected, guide the user through the appropriate print pipeline below.

---

## Text-to-3D Print Pipeline

| Step | Action | Credits | Notes |
|------|--------|---------|-------|
| 1. Preview | Text to 3D (`mode: "preview"`) | 20 | Untextured mesh (white model) |
| 2. Printability Check | Review checklist below | 0 | Manual review |
| 3. Download OBJ | Download model as OBJ | 0 | Slicer-compatible format |
| 4. Open in Slicer | Direct launch or manual import | 0 | See script below |
| 5. (Optional) Multi-color | Refine + color guidance | 10 | See multi-color section |

## Image-to-3D Print Pipeline

| Step | Action | Credits | Notes |
|------|--------|---------|-------|
| 1. Generate | Image to 3D with `should_texture: False` | 20 | Untextured mesh |
| 2. Printability Check | Review checklist below | 0 | Manual review |
| 3. Download OBJ | Download model as OBJ | 0 | Slicer-compatible format |
| 4. Open in Slicer | Direct launch or manual import | 0 | See script below |
| 5. (Optional) Multi-color | Retexture + color guidance | 10 | See multi-color section |

---

## Print Download + Slicer Script

Append this to the reusable script template from `meshy-3d-generation`:

```python
import subprocess, shutil

# After task SUCCEEDED, download OBJ format for printing
obj_url = task["model_urls"].get("obj")
if not obj_url:
    print("OBJ format not available. Available:", list(task["model_urls"].keys()))
    print("Download GLB and import manually into your slicer.")
    obj_url = task["model_urls"].get("glb")

obj_path = os.path.join(project_dir, "model.obj")
download(obj_url, obj_path)
print(f"\nModel ready for printing: {os.path.abspath(obj_path)}")
```

### Opening OBJ in Slicer Software

When the user specifies a slicer (e.g. Bambu Studio, OrcaSlicer, Creality Print, PrusaSlicer, Cura), use the following cross-platform pattern to open the downloaded OBJ file directly:

- **macOS**: `subprocess.run(["open", "-a", "<AppName>", obj_path])`
  - The OS resolves the app location automatically. Use the app display name, e.g. `"BambuStudio"`, `"OrcaSlicer"`, `"PrusaSlicer"`.
- **Windows / Linux**: `shutil.which("<binary_name>")` to find the executable in PATH, then `subprocess.Popen([exe, obj_path])`.
  - If not found in PATH, print the file path and instruct the user to open it manually.

**If the user does NOT specify a slicer**, simply print the OBJ file path and instruct them to open it in their preferred slicer: File → Import / Open → select the .obj file.

Common slicers for reference: Bambu Studio, OrcaSlicer, Creality Print, PrusaSlicer, Cura — but accept any slicer the user names.

---

## Printability Checklist (Manual Review)

> **Note:** Automated printability analysis API is coming soon. For now, manually review the checklist below before printing.

| Check | Recommendation |
|-------|---------------|
| Wall thickness | Minimum 1.2mm for FDM, 0.8mm for resin |
| Overhangs | Keep below 45° or add supports |
| Manifold mesh | Ensure watertight with no holes |
| Minimum detail | At least 0.4mm for FDM, 0.05mm for resin |
| Base stability | Flat base or add brim/raft in slicer |
| Floating parts | All parts connected or printed separately |

**Recommendations**: Import into your slicer to check for mesh errors. Use the slicer's built-in repair tool if needed. Consider hollowing figurines to save material.

---

## Key Rules for Print Workflow

- **Always download OBJ format** for slicer compatibility (3MF not yet available from API)
- **If the user specifies a slicer**, try to open the OBJ file directly using the cross-platform pattern above
- **If the user does NOT specify a slicer**, print the file path and guide them to import manually: File → Import / Open → select .obj file
- After opening in slicer, remind user to check print settings (layer height, infill, supports)
- **If OBJ is not available**: Download GLB and guide user to import manually

---

## Multi-Color Printing (Manual Guidance)

> **Note:** Automated multi-color processing API is coming soon. For now, follow the manual approach below.

1. Use the **Retexture** API (10 credits) to apply distinct color regions to your model
2. Download the model as OBJ
3. In your slicer's color painting tool, assign filament colors to different regions
4. Slice and print with your multi-color setup (e.g., Bambu Lab AMS, Prusa MMU)

For resin printing, consider painting the model after printing for best color results.

---

## Coming Soon

The following features will be integrated into this skill as their APIs launch:

- **Printability Analysis & Fix API** — Automated mesh analysis and repair (non-manifold edges, thin walls, floating parts)
- **Multi-Color Processing API** — Automated color segmentation and filament assignment for AMS/MMU printers

---

## Additional Resources

For the complete API endpoint reference, read [reference.md](reference.md).
