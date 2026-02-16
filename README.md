# Printer Skill for OpenClaw

Print images and PDFs to any CUPS printer. PPD-aware — reads paper sizes, margins, resolution, and duplex from the printer's PPD file at runtime. No hardcoded values.

## Supported File Types

| Format | How it's printed |
|---|---|
| PDF | Sent directly to the printer |
| PNG, JPG, JPEG | Converted to PDF at native printer DPI, centered in printable area |
| GIF, BMP, TIFF, WebP | Same as above (any Pillow-supported image) |

## Commands

### List printers

```bash
python3 scripts/print.py list
python3 scripts/print.py list --json
```

### Print a file

```bash
python3 scripts/print.py print document.pdf
python3 scripts/print.py print photo.png --printer HP_Color_LaserJet
python3 scripts/print.py print document.pdf -o InputSlot=tray-2
python3 scripts/print.py print document.pdf -o cupsPrintQuality=High -o sides=one-sided
```

Pass any CUPS option with `-o KEY=VALUE` (repeatable). Use `options` to discover what's available.

### Query printer info

```bash
python3 scripts/print.py info
python3 scripts/print.py info --json
```

Returns resolution, media size, printable area, margins, trays, and paper sizes:

```json
{
  "printer": "HP_Color_LaserJet_Pro_M478f_9f",
  "manufacturer": "HP",
  "model": "HP Color LaserJet Pro M478f-9f",
  "resolution": "600x600dpi",
  "color": "True",
  "default_paper": "A4",
  "default_duplex": "DuplexNoTumble",
  "trays": ["tray-1", "tray-2", "auto"],
  "paper_sizes": [
    {
      "name": "A4",
      "width_mm": 210.0,
      "height_mm": 297.0,
      "default": true,
      "margins_mm": {
        "left": 4.2,
        "bottom": 4.2,
        "right": 4.2,
        "top": 4.2
      }
    }
  ]
}
```

The `margins_mm` define the non-printable border. The printable area is `width - left - right` by `height - top - bottom`.

### Query CUPS options

```bash
python3 scripts/print.py options
python3 scripts/print.py options --json
```

Shows all available options with their current value and choices. Useful for discovering tray names, quality levels, media types, and duplex modes:

```json
[
  {
    "option": "InputSlot",
    "label": "Media Source",
    "current": null,
    "values": ["tray-1", "tray-2", "auto"]
  },
  {
    "option": "cupsPrintQuality",
    "label": "Quality",
    "current": "Normal",
    "values": ["Draft", "Normal", "High"]
  },
  {
    "option": "Duplex",
    "label": "2-Sided Printing",
    "current": "DuplexNoTumble",
    "values": ["None", "DuplexNoTumble", "DuplexTumble"]
  }
]
```

### Common `-o` options

| Option | Example | Description |
|---|---|---|
| `InputSlot` | `-o InputSlot=tray-2` | Select paper tray |
| `cupsPrintQuality` | `-o cupsPrintQuality=High` | Draft / Normal / High |
| `sides` | `-o sides=one-sided` | Override duplex |
| `ColorModel` | `-o ColorModel=Gray` | Force grayscale |
| `MediaType` | `-o MediaType=cardstock` | Paper type hint |
| `media` | `-o media=Letter` | Override paper size |

## Security

- Files must be inside the workspace or `/tmp` (symlinks are followed, then the resolved path is checked)
- Only printable file types accepted (PDF + common image formats)
- Printer names are validated against `[\w.\-]+` to prevent injection

## Requirements

- Python 3.10+
- CUPS (macOS built-in, Linux: `apt install cups`)
- Pillow (for image printing only)

## License

MIT

## Documentation

- [SKILL.md](SKILL.md) — agent-facing reference (commands, behavior, limitations)
- [SETUP.md](SETUP.md) — prerequisites, configuration, and setup instructions
- [ClawHub](https://www.clawhub.com/skills/cups-printer) — install via ClawHub registry
