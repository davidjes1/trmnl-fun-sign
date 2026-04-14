# TRMNL Fun Sign

A [TRMNL](https://trmnl.com) plugin that turns your e-ink display into a customizable sign.
Type your message in the plugin portal, pick a style, and watch it appear on your screen.

## Features

- **Custom text** - Enter any message to display on your TRMNL
- **Font styles** - Classic, Bold, Compact, or Marquee lettering
- **Border styles** - None, Simple, Double, Dashed, or Thick frames
- **Sign themes** - Standard, Notice Board, Minimalist, or Retro looks
- **Text alignment** - Center, Left, or Right
- **Optional subtitle** - Add an attribution or secondary line
- **Date stamp** - Optionally show the current date on your sign
- **QR code** - Add a scannable QR code linking to any URL
- **All layouts** - Full, Half Horizontal, Half Vertical, and Quadrant
- **Portrait support** - Responsive font scaling for portrait orientation

## How It Works

This is a **static** plugin -- no server or API required. You configure everything
through the TRMNL plugin portal using custom fields, and TRMNL's Liquid templating
engine renders your sign directly.

## Setup

### Install on TRMNL

1. Add this plugin from the TRMNL marketplace (or create a Private Plugin)
2. Configure your sign message and style options
3. Save and wait for your next screen refresh

### Local Development

Requires [trmnlp](https://github.com/usetrmnl/trmnlp) for local preview.

```bash
# Install trmnlp
gem install trmnl_preview

# Start the dev server
trmnlp serve
```

Or with Docker:

```bash
docker run --publish 4567:4567 --volume "$(pwd):/plugin" trmnl/trmnlp serve
```

Open `http://localhost:4567` to preview your plugin in the browser.

## Configuration Options

| Field | Type | Description |
|---|---|---|
| Sign Message | Long text | The main text to display (required) |
| Subtitle | Text | Optional secondary line below the message |
| Font Style | Select | Classic, Bold, Compact, or Marquee |
| Border Style | Select | None, Simple, Double, Dashed, or Thick |
| Text Alignment | Select | Center, Left, or Right |
| Sign Theme | Select | Standard, Notice Board, Minimalist, or Retro |
| Show Date | Select | Display the current date on the sign (Yes/No) |
| QR Code URL | URL | Optional URL to render as a scannable QR code |

## Layouts

| Layout | Best For |
|---|---|
| Full (780x460) | Longer messages, maximum impact |
| Half Horizontal (780x225) | Short messages, wide format |
| Half Vertical (400x460) | Vertical signs, portrait style |
| Quadrant (400x235) | Very short messages, compact |

## Project Structure

```
trmnl-fun-sign/
├── templates/           # Liquid templates for each layout
│   ├── shared.liquid    # Shared styles and components
│   ├── full.liquid
│   ├── half_horizontal.liquid
│   ├── half_vertical.liquid
│   └── quadrant.liquid
├── settings.yml         # Plugin strategy configuration
├── custom-fields.yml    # User-facing form fields
├── .trmnlp.yml          # Local dev server config
└── assets/              # Icons and demo screenshots
```

## Documentation

- [PLAN.md](PLAN.md) - Project roadmap and feature plans
- [ARCHITECTURE.md](ARCHITECTURE.md) - Technical architecture details

## Contributing

Contributions welcome! Feel free to open issues or submit pull requests.

## License

MIT License - see [LICENSE](LICENSE) for details.
