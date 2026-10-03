# LyX macOS → VS Code + LaTeX Workshop migration pack

Baseline: LyX 2.5.3 macOS default binding family. In `mac.bind`, LyX maps `C` to Command, `A` to Option, `M` to Control, and `S` to Shift.

## What this pack does

- Preserves the high-frequency LyX math muscle memory where VS Code can represent it exactly.
- `⌘M`: inline math.
- `⇧⌘M`: display math.
- `⌃M → F/S/X/E/U/I/Y/P/R/...`: fraction, root, sub/superscript, sum, integral, etc.
- Adds fast 2×2 / 3×3 matrices, `cases`, `align`, and `equation`.
- Gives Greek letters mnemonic snippets such as `;ga` + Tab → `\alpha` and `;gD` + Tab → `\Delta`.
- Enables near-live PDF rebuilding with LaTeX Workshop (`onFileChange`) plus VS Code Auto Save.

## Why Greek letters are snippets instead of exact LyX keys

LyX uses sequences such as `M-m g a`, which on macOS means `⌃M → G → A`. VS Code's native chord shortcuts consist of two key combinations, so a three-stroke LyX sequence cannot be reproduced 1:1 with only `keybindings.json`. The package therefore keeps all two-stroke LyX math bindings and uses short snippets for the three-stroke Greek family.

## Installation

1. Install **LaTeX Workshop** in VS Code and make sure MacTeX/TeX Live works from your shell.
2. Open **Command Palette → Preferences: Open Keyboard Shortcuts (JSON)** and merge the contents of `keybindings.json` into your existing array. If you have no custom shortcuts, you can use the file as-is.
3. Open **Command Palette → Snippets: Configure Snippets → latex.json**. Copy the object entries from `latex.code-snippets` into that file. (The `.code-snippets` file can also be placed in your VS Code user snippets directory.)
4. Merge `settings.json` into your User Settings JSON.
5. Open a `.tex` file and run **LaTeX Workshop: View LaTeX PDF file** once. Put the editor and PDF tab side by side.

## Main shortcuts

| LyX habit | VS Code in this pack | Result |
|---|---|---|
| ⌘M | ⌘M | inline `\( … \)` |
| ⇧⌘M | ⇧⌘M | display `\[ … \]` |
| ⌃M → F | ⌃M → F | `\frac{…}{…}` |
| ⌃M → S | ⌃M → S | `\sqrt{…}` |
| ⌃M → X | ⌃M → X | subscript |
| ⌃M → E | ⌃M → E | superscript |
| ⌃M → U | ⌃M → U | sum with limits |
| ⌃M → I | ⌃M → I | integral with limits |
| ⌃M → Y | ⌃M → Y | contour integral |
| ⌃M → P | ⌃M → P | `\partial` |
| ⌃M → R | ⌃M → R | nth root |
| ⌃M → 8 | ⌃M → 8 | `\infty` |
| — | ⌃M → 2 | 2×2 `pmatrix` |
| — | ⌃M → 3 | 3×3 `pmatrix` |
| — | ⌃M → C | `cases` |
| — | ⌃M → A | `align` |

After insertion, press **Tab** to move through the fields. This is the closest VS Code analogue to moving among LyX formula cells.

## Greek letters

Type `;g` + LyX's Greek mnemonic + Tab. Examples:

- `;ga` → `\alpha`
- `;gb` → `\beta`
- `;gv` → `\theta`
- `;gD` → `\Delta`
- `;gO` → `\Omega`

## Matrices and tables

For matrices, `⌃M → 2` and `⌃M → 3` give editable cells with Tab stops. For tables, use `;tab2` or `;tab3`. For large tables, a GUI/clipboard-to-LaTeX table tool remains more convenient than raw source editing; this is the one area where LyX's structural editor remains materially better.

## Live compile settings

`settings.json` uses:

- Auto Save after 500 ms.
- LaTeX Workshop build on file change.
- Minimum auto-build interval 1000 ms.
- Internal PDF viewer in a VS Code tab.
- Automatic SyncTeX jumping after each build is disabled; use `⌘⌥J` when you want to jump to the corresponding PDF position.

The LaTeX Workshop default for `latex-workshop.latex.autoBuild.run` is currently `onFileChange`, and the extension documents a 1000 ms default minimum interval.

## Source inventory

For the effective macOS command set, this pack parses the global `site.bind` base plus the macOS profile and the two command files loaded by it: `mac.bind`, `math.bind`, and `menus.bind`. That produces **433 command bindings**: 24 + 176 + 124 + 109. The 2.5.3 archive also contains `latinkeys.bind` (355), `cyrkeys.bind` (139), and `greekkeys.bind` (75); those are primarily direct keyboard key-symbol `self-insert` mappings rather than editor commands, so they are catalogued in the source inventory but intentionally not mixed into the shortcut-command table.

Files included:

- `lyx_mac_2.5.3_all_command_shortcuts.csv`: all 433 command bindings, with macOS-readable modifier translation and category.
- `lyx_2.5.3_binding_source_inventory.csv`: source-file counts and roles.
- `lyx_to_vscode_mapping.csv`: the writing/math subset, with migration status and VS Code equivalent where supplied.
- `keybindings.json`: directly usable LyX-style VS Code shortcuts.
- `latex.code-snippets`: Greek mnemonics plus matrix/cases/align/table snippets.
- `settings.json`: near-live LaTeX Workshop build settings.

The Greek snippet set follows the LyX source mnemonics, including the source aliases `I→\iota` and `J→\epsilon`.

## Notes

- `⌘M` is traditionally the macOS Minimize shortcut. LyX itself overrides it. VS Code may show a conflict depending on your macOS/VS Code version; user keybindings normally have high priority in the editor, but if the OS/window menu still captures it, use `⌃⌘M` or another key for inline math.
- `⇧⌘M` normally opens the Problems panel in VS Code. The LaTeX-specific `when` condition in this pack overrides it while a LaTeX editor has focus.
- The generated LaTeX is standard `.tex`; no LyX conversion layer is involved.
