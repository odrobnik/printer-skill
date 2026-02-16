# Printer Skill for OpenClaw

Print images and PDFs to any CUPS printer. PPD-aware: reads paper sizes, margins, resolution, and duplex settings from the printer's PPD file at runtime — no hardcoded values.

## Features

- **PDF printing** — sent directly to the printer with correct media settings
- **Image printing** — auto-converts to PDF at the printer's native DPI, centered within the printable area
- **PPD-aware** — reads paper size, imageable area, resolution, and duplex from the PPD file
- **JSON output** — all commands support `--json` for machine-readable output
- **Multi-printer** — uses system default, override per-command with `--printer`

## Commands

```bash
# List printers
uv run scripts/print.py list
uv run scripts/print.py list --json

# Print a file
uv run scripts/print.py print document.pdf
uv run scripts/print.py print photo.png --printer HP_Color_LaserJet

# Printer info (specs, paper sizes, margins)
uv run scripts/print.py info
uv run scripts/print.py info --json

# Printer options (all CUPS options with current values)
uv run scripts/print.py options
```

## Requirements

- Python 3.10+
- CUPS (macOS built-in, Linux: `apt install cups`)
- Pillow (for image printing only)

## License

MIT
