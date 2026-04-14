# TRMNL Fun Sign - Project Plan

## Vision

A TRMNL plugin that turns your e-ink display into a customizable sign. Users type a
message in the TRMNL plugin portal and it renders beautifully on their display with
selectable fonts, borders, themes, and layout options.

No backend server required -- this is a pure static/template plugin powered entirely
by TRMNL's Liquid templating engine and the TRMNL Design System framework.

## Core Features

### 1. Custom Text Display
- User enters a main message via the plugin portal custom fields
- Optional subtitle/attribution line (e.g. "- The Management")
- Character limits enforced per layout size for optimal readability

### 2. Font Styles
Selectable font presentation styles:
- **Classic** - Clean, standard title typography
- **Bold** - Extra-large, high-impact text for short messages
- **Compact** - Smaller text allowing longer messages
- **Marquee** - All-caps bold lettering, event-poster style

### 3. Border Styles
Decorative frame options:
- **None** - Clean, borderless display
- **Simple** - Single-line frame
- **Double** - Double-line frame
- **Dashed** - Dashed border
- **Thick** - Heavy solid border

### 4. Text Alignment
- Center (default)
- Left
- Right

### 5. Sign Themes
Pre-designed visual treatments:
- **Standard** - Clean and professional
- **Notice Board** - Styled like a posted notice with a header bar
- **Minimalist** - Maximum whitespace, understated elegance
- **Retro** - Decorative border pattern, classic sign feel

## Layout Support

All four TRMNL layout sizes are supported:
| Layout | Dimensions | Max Suggested Characters |
|---|---|---|
| Full | 780x460 | ~200 |
| Half Horizontal | 780x225 | ~100 |
| Half Vertical | 400x460 | ~120 |
| Quadrant | 400x235 | ~60 |

## Technical Approach

### Strategy: Static
Since all content comes from user-configured custom fields (no external data source),
this plugin uses the `static` strategy. No polling URL or webhook server is needed.

### Data Flow
1. User configures plugin in TRMNL portal (enters text, selects options)
2. Custom field values are stored by TRMNL
3. On each screen refresh, TRMNL merges custom field values into Liquid templates
4. Rendered HTML is converted to an image and sent to the e-ink display

### Custom Fields
| Field | Type | Description |
|---|---|---|
| `message` | long_text | Main sign text |
| `subtitle` | text | Optional subtitle or attribution |
| `font_style` | select | Typography style |
| `border_style` | select | Border decoration |
| `text_align` | select | Text alignment |
| `sign_theme` | select | Overall visual theme |

### Technology Stack
- **Liquid** - Shopify's templating language (used by TRMNL)
- **TRMNL Design System** - CSS framework optimized for e-ink
- **trmnlp** - Local development server for preview and testing

## Development Phases

### Phase 1 - Foundation (Current)
- [x] Project planning and architecture documentation
- [x] Repository setup with .gitignore, README, LICENSE
- [x] Plugin configuration (settings.yml, custom-fields.yml)
- [x] Initial Liquid templates for all four layouts
- [x] Local dev server configuration (.trmnlp.yml)

### Phase 2 - Polish
- [ ] Test across all layout sizes and orientations
- [ ] Refine typography and spacing for e-ink readability
- [ ] Add demo screenshots to assets/demo/
- [ ] Create plugin icon for assets/icon/

### Phase 3 - Publish
- [ ] Submit to TRMNL plugin marketplace or publish as a Recipe
- [ ] Write setup guide for users
- [ ] Gather community feedback

## Future Feature Ideas
- Date/time stamp option on the sign
- QR code integration (link visitors to a URL)
- Rotating messages (multiple messages cycling)
- Emoji/icon support (where e-ink rendering allows)
- Seasonal/holiday border themes
