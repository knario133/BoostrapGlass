# Corporate Glassmorphism Theme

This document describes the architectural decisions, modifications, and usage of the Corporate Glassmorphism theme implemented on top of Bootstrap.

## Architecture Implemented
We implemented an architecture emphasizing a clean separation between Bootstrap's original SCSS architecture and our customized "glassmorphism" variables, ensuring simple updates or disabling while guaranteeing maximum effect.

The custom theme resides in `scss/theme/` and `scss/integrations/`:
1. `_glass-variables.scss`: Centralized glassmorphism design tokens (colors, blurs, opacities, and depths) that declare default variables via `!default`. It is injected prior to bootstrap's `_variables.scss` to allow proper overriding.
2. `_glass-mixins.scss`: Reusable multi-level shadow mixes, text-reliefs, and glass-surface backgrounds with `backdrop-filter`.
3. `_glass-root.scss`: Applies a minimal and unobtrusive fixed radial gradient backgdrop on the `<body>` element to accentuate the glass transparency.
4. `_glass-components.scss`: Directly modifies and targets `.card`, `.form-control`, `.btn`, dropdowns, switches, checks, radios, and structural input groups by utilizing the mixins above. Added to the bottom of `bootstrap.scss` to ensure the cascades overwrite default declarations.
5. `integrations/_fontawesome-glass.scss`: An optional module giving font-awesome icons depth and highlighting matching semantic states (`.text-success`, etc.).
6. `scss/bootstrap-fontawesome.scss`: An optional entrypoint allowing you to build the core bootstrap styles + the font awesome integrations out of the box in `dist/css/bootstrap-fontawesome.css`.

## Modified Files
* `scss/bootstrap.scss`: Modified to integrate the SCSS files in the proper order.

## New Files
* `scss/theme/_glass-variables.scss`
* `scss/theme/_glass-mixins.scss`
* `scss/theme/_glass-root.scss`
* `scss/theme/_glass-components.scss`
* `scss/integrations/_fontawesome-glass.scss`
* `scss/bootstrap-fontawesome.scss`
* `examples/glassmorphism-controls/index.html`

## Core Variables
All core variables are located in `scss/theme/_glass-variables.scss`.
- Primary colors (`$glass-color-primary`, `$glass-color-focus`)
- Blur (`$glass-blur`: 20px)
- Shadows (`$glass-shadow-outer`, `$glass-shadow-inner`)
- Surface colors (`$glass-bg-light`, `$glass-bg-heavy`)

## How to Compile
You can compile this theme by using Bootstrap's default commands:
```sh
# Clean install of dependencies
npm ci

# Compile CSS (including the new bootstrap-fontawesome.scss)
npm run css-compile

# Generate everything (prefix, minify, rtl)
npm run css

# Generate JS files
npm run js
```

## How to use the theme
The theme is integrated deeply into `bootstrap.scss`. After running compilation, any project requesting `dist/css/bootstrap.css` or `dist/css/bootstrap.min.css` will inherit the glassmorphic behavior natively by just declaring standard elements (e.g., `<div class="card"></div>` or `<input type="text" class="form-control">`).

## How to change Colors and Depth
To alter the corporate colors or glass depths, edit the values in `scss/theme/_glass-variables.scss`.
- Depth: Modify `$glass-shadow-outer` and `$glass-shadow-inner`.
- Blur/Glassiness: Modify `$glass-blur` and `$glass-saturation`.

## Activating Font Awesome Integration
The Font Awesome integration is entirely optional and does not break the standard API.
1. The developer must load Font Awesome's CDN or CSS independently.
2. Include the pre-compiled `dist/css/bootstrap-fontawesome.css` (which includes both Bootstrap and the icon styles) OR manually import `@import "integrations/fontawesome-glass";` in their own SCSS after Bootstrap is loaded.

## Compatibility & Fallback
Browsers without support for `backdrop-filter` or `-webkit-backdrop-filter` will automatically revert to a more opaque background variation, `$glass-bg-solid` (e.g. `rgba(255, 255, 255, 0.9)`). Depth shadows and illuminated borders are retained even when blur is unavailable, ensuring the components don't become flat white squares.

## Limitations
* Because the transparency effect depends on the background to create depth, it pairs best with lighter and white backgrounds.
* Focus glowing utilizes `box-shadow`, so developers should be careful when altering outline behaviors.
* Reduced Motion (`prefers-reduced-motion`) will disable button lifting/pressing simulations for accessibility.

## Validation / Testing Results
* `npm run css-lint`: PASS (All unused variables removed; all styling violations natively overridden).
* `npm run test`: All testing passed (excluding unrelated legacy javascript TODO comments naturally present in the fork).
* The demo located at `examples/glassmorphism-controls/index.html` renders effectively across sizes matching the design criteria.