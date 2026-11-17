# gpui-pre-wgpu (crabber)

[`gpui-pre-wgpu`](https://crates.io/crates/gpui-pre-wgpu) 0.3.12 from crates.io, the published
snapshot of Zed's `gpui_wgpu` crate, with one patch for
[crabber](https://github.com/auf-gg/crabber). Each release is imported as published, with the
patch kept on top. crabber wires it in through `[patch.crates-io]`.

## Colour emoji on Linux (`src/cosmic_text_system.rs`)

With Noto Color Emoji installed, emoji came out black and white whenever another installed font
had them (DejaVu Sans, Noto Sans Symbols, Nerd Fonts), and an emoji written with U+FE0F (❤️, ©️,
1️⃣) used the plain glyph of the UI font.

1. `load_family`: a face without the letter `m` is not dropped if it is the known colour emoji
   font, so it can be loaded as a family at all.
2. `with_emoji_fallback`: Noto Color Emoji joins every font's fallback chain, ahead of
   cosmic-text's own fallbacks. gpui otherwise drops the app's fallbacks when the requested family
   is missing (the default `.SystemUIFont` maps to IBM Plex Sans).
3. `emoji_slot`: a grapheme with U+FE0F or U+20E3 goes to the emoji font even where the primary
   font has a glyph for its first character.

Windows is not affected: it renders text through DirectWrite (`gpui-pre-windows`).

Upstream: the same change for Zed's `crates/gpui_wgpu` is on the
[`gpui-wgpu-linux-color-emoji`](https://github.com/auf-gg/zed/tree/gpui-wgpu-linux-color-emoji)
branch of our Zed fork.

## License

Apache-2.0, as the original crate (`LICENSE-APACHE`).
