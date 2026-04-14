# Contributing to TRMNL Fun Sign

## Local Development

1. Install [trmnlp](https://github.com/usetrmnl/trmnlp):
   ```bash
   gem install trmnl_preview
   ```

2. Start the dev server:
   ```bash
   trmnlp serve
   ```

3. Open `http://localhost:4567` in your browser.

4. Edit templates in `templates/` -- the preview auto-reloads.

## Customizing Dev Preview Data

Edit `.trmnlp.yml` to change the preview values for custom fields:

```yaml
custom_fields:
  message: "Your test message here"
  subtitle: "- Test Author"
  font_style: "bold"
  border_style: "double"
  text_align: "center"
  sign_theme: "retro"
```

## Template Guidelines

- All shared CSS lives in `templates/shared.liquid`
- Each layout file handles one TRMNL layout size
- Always provide a fallback for empty/missing `message` field
- Test your changes across all four layout sizes
- Keep e-ink constraints in mind: no color, high contrast preferred
