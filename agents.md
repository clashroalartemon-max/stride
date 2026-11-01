# AGENTS.md

- The whole app is `index.html`: one `<style>` block, the markup, and one inline script. There is no bundler.
- Rendering rule: never use `backdrop-filter`. A global rule at the end of the stylesheet forces it off, because it caused colored stripes and ripples on iOS Safari and Android Chrome during scrolling and tab switches. Fake the "glass" look with semi-opaque backgrounds instead.
- `.glass` cards sit on their own GPU layers (`translateZ(0)`). Don't put `position: fixed` elements inside a `.glass` card, because the transform would trap them.
- The backdrop blobs are radial gradients, not `filter: blur()`. Keep them that way.
- Premium shop backgrounds re-theme the whole app through `BG_TO_THEME` → `.backdrop.theme-*` classes. The VIP background (`b-vip`) maps to `theme-admin`.
