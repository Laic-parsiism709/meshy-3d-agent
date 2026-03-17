# Meshy Agent Skill

AI agent skills for the [Meshy AI](https://www.meshy.ai) 3D generation platform. Enables AI coding assistants (Cursor, Claude Code) to generate 3D models, textures, images, rig characters, animate them, and prepare models for 3D printing — no MCP server required.

## How It Works

These are **pure Markdown skills** — no server, no dependencies, no build step. Your AI assistant reads the skill files and gains the ability to interact with the Meshy API directly using shell commands and Python scripts.

## Skills

### `meshy-3d-generation` (core)

Full 3D generation lifecycle: API key setup, task creation, polling, downloading, and multi-step pipelines.

| Capability | Description | Credits |
|-----------|-------------|---------|
| Text to 3D | Generate 3D models from text descriptions | 20-30 |
| Image to 3D | Convert single or multiple images to 3D | 20-30 |
| Retexture | Apply new textures to existing models | 10 |
| Remesh | Change topology, polycount, or export format | 5 |
| Auto-Rigging | Add skeleton to humanoid characters (includes walking + running) | 5 |
| Animation | Apply custom animations to rigged characters | 3 |
| Text to Image | Generate 2D images from text | 3-9 |
| Image to Image | Transform existing images | 3-9 |

### `meshy-3d-printing` (optional)

3D printing workflow: printability checks, slicer integration (Bambu Studio), multi-color guidance.

| Capability | Description | Credits |
|-----------|-------------|---------|
| Print Pipeline | Text/Image to 3D → OBJ download → Slicer | 20 |
| Printability Check | Wall thickness, overhangs, manifold mesh review | 0 |
| Slicer Integration | Open in Bambu Studio via URL scheme | 0 |
| Multi-Color Guidance | Color segmentation for AMS/MMU printers | 0-10 |

> Install the generation skill for full functionality. The printing skill depends on its script template and environment setup.

## Installation

### Prerequisites

- A Meshy API key ([get one here](https://www.meshy.ai/settings/api) — requires Pro plan or above)
- Python 3 with `requests` package (`pip install requests`)

### Cursor

```bash
mkdir -p .cursor/skills

# Core (required)
cp meshy-3d-generation.md .cursor/skills/
cp reference.md .cursor/skills/meshy-reference.md

# 3D Printing (optional)
cp meshy-3d-printing.md .cursor/skills/
```

Set your API key:

```bash
export MESHY_API_KEY="msy_YOUR_API_KEY"
```

### Claude Code

```bash
mkdir -p .claude/skills

# Core (required)
cp meshy-3d-generation.md .claude/skills/
cp reference.md .claude/skills/meshy-reference.md

# 3D Printing (optional)
cp meshy-3d-printing.md .claude/skills/
```

Set your API key:

```bash
export MESHY_API_KEY="msy_YOUR_API_KEY"
```

### Test Mode

Use the test API key to try without credits:

```bash
export MESHY_API_KEY="msy_dummy_api_key_for_test_mode_12345678"
```

## Skill vs MCP Server

| Feature | Agent Skill (this repo) | [MCP Server](https://github.com/Arlieeee/meshy-mcp-server) |
|---------|------------------------|-------------------------------------------------------------|
| Setup | Copy Markdown files | `npx meshy-mcp-server` |
| Dependencies | Python 3 + requests | Node.js >= 18 |
| How it works | AI reads instructions, makes API calls directly | Dedicated server process with structured tools |
| IDE support | Cursor, Claude Code | Any MCP-compatible client |
| File management | Via skill instructions | Built-in auto-save with project folders |
| Best for | Quick setup, no Node.js needed | Structured tool integration, Smithery registry |

Both approaches provide the same Meshy API capabilities. Choose based on your preference and setup.

## Files

| File | Description |
|------|-------------|
| `meshy-3d-generation.md` | Core skill: environment setup, all generation workflows, rigging, animation |
| `meshy-3d-printing.md` | Print skill: printability checks, slicer integration, multi-color guidance |
| `reference.md` | Complete Meshy API reference with all endpoints, parameters, and examples |

## License

[MIT](LICENSE)
