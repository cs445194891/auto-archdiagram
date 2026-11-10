[简体中文](README.md) · **English**

# auto-archdiagram

> Turn a structured description — or a pasted paragraph of Chinese prose — into a printable, fully editable layered architecture diagram. One HTML file, zero dependencies, fully offline.

**v0.1.0** · MIT License · 100% offline · zero dependencies · no build step · Simplified Chinese UI

Built for the architecture chapters of Chinese project proposals and feasibility studies: white background, dark text, safe for black-and-white printing, with every label still editable. It renders a stack of layers, optional left/right cross-cutting support bands, an optional bottom process flow, and a roof-style or plain-text introduction.

---

## Quick start

```bash
git clone https://github.com/cs445194891/auto-archdiagram.git
cd auto-archdiagram
open auto-archdiagram.html        # macOS
# Windows: start auto-archdiagram.html
# Linux:   xdg-open auto-archdiagram.html
```

Or just download `auto-archdiagram.html` and double-click it. No server, no install, no account, no network — the entire tool is that single file.

> The interface is in Simplified Chinese and exported files use Chinese default filenames. The paste parser is tuned for Chinese construction descriptions; every other label can be typed in any language.

## Highlights

- **Genuinely offline.** No CDN, no external fonts or scripts, no telemetry, no network requests. Everything — including ZIP and OOXML generation — is implemented inline.
- **Three input paths.** A structured form, rule-based paste-and-split for Chinese prose, and an editable review table with per-field provenance. Unrecognised paragraphs are listed separately: nothing is silently invented or filled in.
- **Click-to-edit on the canvas.** Select any text in the preview to change its content, font size and alignment. The selection survives consecutive adjustments; restore one block or reset everything.
- **Layout control.** Up to 5 layers; left / right / both cross-cutting bands; an optional bottom flow band; roof-style or plain-text intro; 5 light themes, all white background / dark text and safe for black-and-white printing.
- **Per-layer height (64–400 px)** with automatic content compression down to 0.75×. When a support band is taller, layer gaps widen instead of stretching layer frames, so bottom edges stay aligned.
- **Even vertical distribution.** Content blocks inside every frame are re-laid out with equal top/bottom padding and evenly distributed gaps — no large empty space under a frame's content.
- **Item-level editing.** Component cards, feature chips, support-band group items and flow steps can each be added, deleted and edited independently. Expanded groups stay open after deletions.
- **Built-in self-check** reports text overflow, frame balance, actual layer heights, band alignment and dark fills before export.

## Data model

Import or export the whole diagram as JSON (`导出模型 JSON` / `导入模型 JSON`); a provenance CSV is available as well.

```js
// structure sketch, not literal JSON
{
  canvasW, introStyle, roofAngle, roofH, theme,
  title, subtitle, note, captions: [text, text],
  layers: [{ name, place, dashed, h, cards: [{ t, s }], chips: [text], lines: [text] }],
  cross: {
    side: 'none' | 'left' | 'right' | 'both',
    right: { name, sub1, sub2, capLabel, caps: [], withLabel, withItems: [], sysLabel, system: [] },
    left:  { /* same shape, independent content */ }
  },
  flow: { enabled, arrows, title, right, foot, steps: [{ name, lines: [text] }] }
}
```

Limits: 5 layers · 4 component cards and 9 feature chips per layer · 8 items per support-band group · 6 flow steps · 3 lines per step · canvas width 600–1200 px.

## Export formats

| Output | Editable afterwards | Notes |
| --- | --- | --- |
| `SVG` | In vector editors; in newer Office via **Convert to Shape** | White background, print-ready |
| `PNG` | No | 2× resolution |
| `Office-friendly SVG` | Yes, in newer PowerPoint / WPS | Groups flattened, markers expanded into explicit triangles, `<defs>` removed |
| `PPTX` | Yes — native PowerPoint shapes | Generated in-browser as a minimal OOXML/ZIP package; frames, text runs and connectors are separate shapes |
| `draw.io` (`.drawio`) | Yes, in diagrams.net | Can be saved as `.vsdx` for Visio |
| `Editable HTML` | Yes — standalone file | Carries its own inline editor; does not re-expose the PPTX / Office-SVG / draw.io buttons |
| `CSV` / `JSON` | Data, not graphics | Provenance table / full model round-trip |
| Print | — | Prints only the diagram and resets zoom |

### Editing in Office and Visio

Per Microsoft and WPS documentation, PowerPoint 2019 / 2021 / 2024 / Microsoft 365 (Windows) and WPS Presentation 12.1.0.28022+ can convert an inserted SVG into individual editable shapes. PowerPoint 2016 and earlier, and Mac / mobile versions, treat it as a single picture — use PNG or the native PPTX export there. Visio normally keeps an inserted SVG as a picture; use the draw.io export and save as `.vsdx`.

**Verification status:** the statements above come from vendor documentation, not from opening these files in real Office/WPS/Visio on the development machine. The PPTX master/layout/theme is intentionally minimal. Please open the exported files once in your own Office/WPS/Visio before relying on them.

## Privacy

Pasted text stays in memory only. Templates are written to the browser's `localStorage` **only** when you explicitly click **保存模板**. There is no analytics, no upload and no external resource of any kind.

## Known limitations (v0.1.0)

- The UI and the built-in example are in Chinese. The example describes a generic video-conferencing system and contains no real organisation names or project data.
- The paste parser is rule-based and tuned for the "layer description + enumeration + process verbs" style of Chinese construction text. Other prose needs manual editing.
- A custom layer height below that layer's compressed minimum is raised to the minimum with a warning. A single layer combined with a support band is still stretched to keep bottom edges aligned.
- `.drawio` is **not** legacy Visio `.vdx`; use diagrams.net → *Export as → VSDX*.
- Hiding flow arrows leaves the 10 px gaps in place; step cards do not widen.
- Exported default filenames are in Chinese.
- Verified with headless Chrome (geometry, XML, export packages, synthetic interactions). Other modern browsers use only standard SVG / Canvas / Blob APIs but have **not** been verified.

## Contributing

This is an early release; the data model and UI may still change. Issues and pull requests are welcome. For a bug report, please attach the exported model JSON and the steps to reproduce.

## License

Licensed under the [MIT License](LICENSE).
