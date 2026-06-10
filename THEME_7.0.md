# Theme 7.0.3

This repository's latest public theme snapshot is [`Theme-Enterprise_7.0.3.vth`](./Theme-Enterprise_7.0.3.vth).

## Snapshot

- VL header: `// VL_VERSION:4.2.2`
- Theme root: `<Theme-Enterprise-7.0>`
- Theme version: `7.0.3`
- Style space version: `2.0`
- Base theme: `Platform/Theme-Default-Light@1`
- Profile: `enterprise`

## What Is Included

Theme 7.0.3 is a point-slot based design token contract for current VL projects. The current file is organized around these slot families:

| Family | Purpose |
| --- | --- |
| `intent.*` | semantic color intent such as primary, success, warning, danger |
| `emphasis.*` | filled, outlined, tonal, ghost, and text presentation styles |
| `shape.*` | radius variants such as default, pill, soft, and sharp |
| `surface.*` | panel/background tokens for solid, subtle, elevated, overlay, and dark surfaces |
| `textRole.*` | body, caption, hint, muted, weak, and contrast text roles |
| `size.*` | small, medium, and large size scales |
| `affordance.*` | hover, focus, active, and selected interaction behavior |

## Practical Usage

Typical project layout:

```text
ProjectName/
├── Apps/
├── Sections/
├── Services/
├── Database/
└── Theme/
    └── Theme-Enterprise_7.0.3.vth
```

Recommended pairing:

- use a consistent `VL_VERSION` across `.vx`, `.sc`, `.cp`, `.vs`, `.vdb`, and `.vth`
- keep the theme file in the project's `Theme/` directory
- reference the theme from the app entry according to your project conventions

## Why This Matters

The newer sample packs in [`Examples/`](./Examples/README.md) were produced against the same generation baseline:

- current VL source
- Theme 7.0 token model
- bundled `appCaseJsonMap` snapshots for easier inspection in VLC

## Related Files

- [VL 4.3.1 Reference](./VL_VERSION_4.3.1.md)
- [Theme-Enterprise_7.0.3.vth](./Theme-Enterprise_7.0.3.vth)
- [Example Packs](./Examples/README.md)
