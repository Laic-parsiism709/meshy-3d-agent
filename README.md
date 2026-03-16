# Meshy Agent Skill

AI agent skill for the [Meshy AI](https://www.meshy.ai) 3D generation platform. Enables AI coding assistants (Cursor, Claude Code) to generate 3D models, textures, images, rig characters, and animate them through direct API calls — no MCP server required.

## How It Works

This is a **pure Markdown skill** — no server, no dependencies, no build step. Your AI assistant reads the skill file and gains the ability to interact with the Meshy API directly using shell commands and Python scripts.

The skill handles:
- API key detection and setup
- Task creation, polling with exponential backoff, and downloading
- Multi-step pipelines (preview → refine → rig → animate)
- Smart file organization in `meshy_output/` directory
- Cost awareness and credit tracking
- 3D printing workflows

## Capabilities

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
| 3D Printing | Download for printing, open in Bambu Studio | 0 |

## Installation

### Prerequisites

- A Meshy API key ([get one here](https://www.meshy.ai/settings/api) — requires Pro plan or above)
- Python 3 with `requests` package (`pip install requests`)

### Cursor

1. Create the skills directory in your project:
   ```bash
   mkdir -p .cursor/skills
   ```

2. Copy the skill files:
   ```bash
   cp SKILL.md .cursor/skills/meshy-api-skill.md
   cp reference.md .cursor/skills/meshy-reference.md
   ```

3. Set your API key:
   ```bash
   export MESHY_API_KEY="msy_YOUR_API_KEY"
   ```

4. Ask Cursor to generate 3D models — it will automatically use the skill.

### Claude Code

1. Create the skills directory in your project:
   ```bash
   mkdir -p .claude/skills
   ```

2. Copy the skill files:
   ```bash
   cp SKILL.md .claude/skills/meshy-api-skill.md
   cp reference.md .claude/skills/meshy-reference.md
   ```

3. Set your API key:
   ```bash
   export MESHY_API_KEY="msy_YOUR_API_KEY"
   ```

4. Ask Claude Code to generate 3D models — it will automatically use the skill.

### Test Mode

Use the test API key to try without credits:

```bash
export MESHY_API_KEY="msy_dummy_api_key_for_test_mode_12345678"
```

## Skill vs MCP Server

| Feature | Agent Skill (this repo) | [MCP Server](https://github.com/Arlieeee/meshy-mcp-server) |
|---------|------------------------|-------------------------------------------------------------|
| Setup | Copy 2 Markdown files | `npx meshy-mcp-server` |
| Dependencies | Python 3 + requests | Node.js >= 18 |
| How it works | AI reads instructions, makes API calls directly | Dedicated server process with structured tools |
| IDE support | Cursor, Claude Code | Any MCP-compatible client |
| File management | Via skill instructions | Built-in auto-save with project folders |
| Best for | Quick setup, no Node.js needed | Structured tool integration, Smithery registry |

Both approaches provide the same Meshy API capabilities. Choose based on your preference and setup.

## Files

| File | Description |
|------|-------------|
| `SKILL.md` | Main skill file with workflow instructions, code templates, and 3D printing guidance |
| `reference.md` | Complete Meshy API reference with all endpoints, parameters, and examples |

## License

[MIT](LICENSE)
