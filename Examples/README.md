# Example Packs

These are the current recommended public sample packs for VLC import and local testing.

## Current Packs

| Pack | Focus |
| --- | --- |
| [VL_CourseScheduler_WithCaseJsonMap.zip](./VL_CourseScheduler_WithCaseJsonMap.zip) | timetable grid, conflict badge, editor dialog, partial refresh |
| [VL_HabitCheckin_WithCaseJsonMap.zip](./VL_HabitCheckin_WithCaseJsonMap.zip) | date handling, boolean toggles, stats cards, streak logic |
| [VL_MediaShelf_WithCaseJsonMap.zip](./VL_MediaShelf_WithCaseJsonMap.zip) | media library browsing, detail routing, favorites, mock-first data |
| [VL_ShoppingList_WithCaseJsonMap.zip](./VL_ShoppingList_WithCaseJsonMap.zip) | add/remove rows, quantity stepper, category filters, budget summary |
| [VL_VoteMini_WithCaseJsonMap.zip](./VL_VoteMini_WithCaseJsonMap.zip) | single choice, multi choice, progress states, result aggregation |

## Included in Every Pack

Each zip contains:

- `VLProject/` source files
- `appCaseJsonMap/` runtime snapshots
- project-specific notes under `Process/`

This structure makes the packs easier to inspect in VLC without requiring users to regenerate every runtime artifact from scratch first.

## Import Notes

Recommended flow:

1. Download a zip pack from this directory.
2. Import it into VLC.
3. Inspect the project source under `VLProject/`.
4. Use the bundled `appCaseJsonMap/` data to explore runtime structure and panel mapping.

## Legacy Packs

Older public examples were moved to [`legacy/`](./legacy/README.md). They are kept for reference, but the packs in this folder are the ones we recommend first for current testing.
