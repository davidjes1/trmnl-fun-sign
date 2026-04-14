# TRMNL Fun Sign - Architecture

## System Overview

```
+------------------+       +-----------------+       +------------------+
|  TRMNL Plugin    |       |  TRMNL Server   |       |  TRMNL Device    |
|  Portal (Web UI) | ----> |  (Rendering)    | ----> |  (E-Ink Display) |
|                  |       |                 |       |                  |
|  User enters:    |       |  1. Reads       |       |  Displays        |
|  - Message text  |       |     custom      |       |  rendered sign   |
|  - Font style    |       |     fields      |       |  image           |
|  - Border style  |       |  2. Merges into |       |                  |
|  - Theme         |       |     Liquid      |       |                  |
|  - Alignment     |       |     templates   |       |                  |
|                  |       |  3. Renders to  |       |                  |
|                  |       |     bitmap      |       |                  |
+------------------+       +-----------------+       +------------------+
```

## Plugin Strategy: Static

This plugin uses the **static** strategy, meaning:
- No external API polling or webhook server is required
- All dynamic content comes from user-configured **custom fields**
- TRMNL handles rendering and delivery to the device
- Zero infrastructure cost for the plugin developer or user

## File Structure

```
trmnl-fun-sign/
├── .github/
│   └── TEMPLATE_USAGE.md       # Template contribution notes
├── .trmnlp.yml                 # Local dev server config
├── assets/
│   ├── demo/                   # Screenshot previews
│   └── icon/                   # Plugin marketplace icon
├── custom-fields.yml           # User-facing form field definitions
├── settings.yml                # Plugin strategy & refresh config
├── templates/
│   ├── shared.liquid           # Reusable styles & components
│   ├── full.liquid             # Full-screen layout (780x460)
│   ├── half_horizontal.liquid  # Half horizontal layout (780x225)
│   ├── half_vertical.liquid    # Half vertical layout (400x460)
│   └── quadrant.liquid         # Quadrant layout (400x235)
├── PLAN.md                     # Project roadmap
├── ARCHITECTURE.md             # This file
├── README.md                   # User-facing documentation
├── LICENSE                     # MIT license
└── .gitignore                  # Git ignore rules
```

## Template Architecture

### Layout System

TRMNL supports four layout sizes. Each layout file wraps content in the
appropriate `view` class:

| Layout | CSS Class | Use Case |
|---|---|---|
| Full | `view view--full` | Entire screen, best for long messages |
| Half Horizontal | `view view--half_horizontal` | Top or bottom half, good for short messages |
| Half Vertical | `view view--half_vertical` | Left or right half, good for vertical signs |
| Quadrant | `view view--quadrant` | Quarter screen, best for very short messages |

### Template Data Flow

```
custom-fields.yml          Liquid Templates            E-Ink Bitmap
┌──────────────┐      ┌─────────────────────┐      ┌──────────────┐
│ message       │      │                     │      │              │
│ subtitle      │─────>│  {{ message }}       │─────>│  Rendered    │
│ font_style    │      │  {{ subtitle }}      │      │  Sign Image  │
│ border_style  │      │  {% if/case logic %} │      │              │
│ text_align    │      │                     │      │              │
│ sign_theme    │      │  + shared.liquid     │      │              │
└──────────────┘      │    (CSS styles)      │      └──────────────┘
                      └─────────────────────┘
```

### Shared Component (shared.liquid)

Contains all CSS styles and reusable markup:
- **Font style classes** - CSS for each selectable font presentation
- **Border style classes** - CSS for each border decoration option
- **Theme classes** - CSS for each sign theme
- **Utility classes** - Alignment, padding, and layout helpers
- **Fallback states** - Error display when no message is configured

### Per-Layout Templates

Each layout template:
1. Includes `shared.liquid` for styles
2. Reads custom field values from `{{ trmnl.plugin_settings.custom_fields_values }}`
3. Applies conditional logic for font/border/theme selection
4. Wraps content in the correct `view--*` container
5. Handles the empty-message fallback state

## Custom Fields Schema

```yaml
- key: message
  type: long_text
  label: "Sign Message"
  required: true

- key: subtitle
  type: text
  label: "Subtitle"
  required: false

- key: font_style
  type: select
  label: "Font Style"
  options: [Classic, Bold, Compact, Marquee]

- key: border_style
  type: select
  label: "Border Style"
  options: [None, Simple, Double, Dashed, Thick]

- key: text_align
  type: select
  label: "Text Alignment"
  options: [Center, Left, Right]

- key: sign_theme
  type: select
  label: "Sign Theme"
  options: [Standard, Notice Board, Minimalist, Retro]
```

## Rendering Constraints

### E-Ink Display Considerations
- **No color** - Design uses black, white, and dithered grays only
- **No animation** - Static content only; transitions are not visible
- **Refresh rate** - Screen updates on TRMNL's configurable schedule
- **Resolution** - Target 800x480 pixels (actual varies by device model)
- **Contrast** - High contrast designs work best on e-ink

### Text Length Guidelines
Text is the primary content. Templates handle overflow gracefully:
- Long text wraps naturally within the sign container
- Very long text may be clipped at the container boundary
- Users are guided by suggested character limits in field labels

## Development Workflow

### Local Development
```bash
# Install trmnlp (requires Ruby 3.x)
gem install trmnl_preview

# Or use Docker
docker run --publish 4567:4567 --volume "$(pwd):/plugin" trmnl/trmnlp serve

# Start local dev server
trmnlp serve
# -> Opens browser preview at http://localhost:4567
```

### Testing Checklist
- [ ] All four layouts render correctly
- [ ] Each font style displays as expected
- [ ] Each border style renders properly
- [ ] Each theme applies correctly
- [ ] Empty message shows fallback state
- [ ] Long messages wrap without breaking layout
- [ ] Short messages center properly in available space

### Deployment
```bash
# Login to TRMNL
trmnlp login

# Push plugin to TRMNL
trmnlp push
```
