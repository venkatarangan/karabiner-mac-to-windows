# karabiner-mac-to-windows

> Use your Windows keyboard on macOS exactly like you would on Windows — shortcuts, navigation, and all.

---

## Why this exists

I've been a Windows power user for as long as I can remember. Keyboard shortcuts aren't a convenience for me — they're how I think. `Ctrl+C`, `Ctrl+Z`, `Home`, `End`, `Ctrl+Left` to jump a word, `Win+E` to open Explorer. Deep muscle memory that kicks in before conscious thought.

Two weeks ago I set up a Mac Mini at home for casual use. My work machine is still Windows. So every day I switch between the two — and every day my fingers would reach for shortcuts that either did nothing or did the wrong thing entirely on macOS.

I tried a few existing Karabiner profiles. They helped, but most of them carve out exceptions for Terminal, VS Code, and other developer tools — which is exactly where I need consistent shortcuts the most. I also wanted `Win+E` to open Finder, `Win+Shift+S` to snip a screenshot, `Delete` to move files to bin in Finder, and a handful of other things that no existing profile covered together.

So I built this. It's a Karabiner-Elements ruleset that makes a Windows keyboard behave like Windows across every app on macOS — no exceptions, no surprises.

If you're a Windows user spending time on a Mac and you want your keyboard to just work the way you expect, this is for you.

---

## Tested on

| | |
|---|---|
| **Keyboard** | Microsoft Ergonomic Keyboard |
| **macOS** | macOS Tahoe 26.4.1 |
| **Apps tested** | VS Code, Google Chrome, Vivaldi, Microsoft Word, Finder, WhatsApp, Evernote, Terminal |

Should work on any standard Windows layout keyboard where the Win key registers as `left_command` in macOS. See the troubleshooting section if your Win key is behaving unexpectedly.

---

## What it does differently from other profiles

Most Karabiner Windows-mode profiles exclude Terminal, IDEs, and browsers from some remappings. This profile excludes nothing — behaviour is identical in every app.

| Feature | Other profiles | This profile |
|---|---|---|
| App exclusions | Terminal, IDEs excluded from Ctrl remaps | None — universal |
| VS Code | Often excluded | ✓ Included |
| Terminal | Often excluded | ✓ Included |
| `Win+E` → Finder | Rarely included | ✓ |
| `Win+Shift+S` → Screenshot snip | Not included | ✓ |
| `Win+Shift+F` → Screenshot toolbar | Not included | ✓ |
| `F2` → Rename in Finder | Not included | ✓ |
| `Delete` → Move to bin in Finder | Not included | ✓ |
| `Ctrl+V` → Move files in Finder | Not included | ✓ |
| `Win+D` → Show Desktop | Rarely included | ✓ |
| Full setup guide | Minimal | ✓ Step-by-step docx included |

> **Note on Terminal and `Ctrl+C`:** Because there are no app exclusions, `Ctrl+C` in Terminal copies text rather than sending SIGINT. To interrupt a running process use `Ctrl+F4` to close the tab, or kill the process from another tab or Activity Monitor. If you'd prefer a variant that restores SIGINT in Terminal, open an issue.

---

## Installation

### Step 1 — Install Karabiner-Elements

```bash
brew install --cask karabiner-elements
```

Or download the `.pkg` directly from [karabiner-elements.pqrs.org](https://karabiner-elements.pqrs.org).

---

### Step 2 — Grant macOS permissions

This step is skipped by most people and is the most common reason nothing works. Karabiner needs three separate permissions.

**Input Monitoring**
System Settings → Privacy & Security → Input Monitoring → toggle ON for Karabiner-Elements.
If it is not listed, click + and add it from Applications.

**Accessibility**
System Settings → Privacy & Security → Accessibility → toggle ON for Karabiner-Elements.

**System extension / driver**
Open Karabiner-Elements → System Extensions tab. If it shows "Driver is not loaded", click Enable and approve it in System Settings → Privacy & Security → Security (scroll to the bottom, click Allow). Restart your Mac after enabling.

---

### Step 3 — Complete the keyboard setup assistant

When you plug in a Windows keyboard for the first time, macOS shows a Keyboard Setup Assistant popup. Complete it — do not dismiss it.

1. Press the key to the right of left Shift (that is `Z`)
2. Press the key to the left of right Shift (that is `/`)
3. Select **ANSI**
4. Click Done

If you already dismissed the popup: System Settings → Keyboard → Change Keyboard Type…

---

### Step 4 — Verify your Win key

Open Karabiner-Elements → menu bar → **Launch EventViewer**. Press your physical Win key. The `name` field must show `left_command`.

If it shows `left_option` instead: Karabiner-Elements → Simple Modifications → select your keyboard → delete any `left_command ↔ left_option` swap. Also check System Settings → Keyboard → Keyboard Shortcuts → Modifier Keys and reset to defaults.

---

### Step 5 — Install the ruleset

Download `json/windows_shortcuts_ms_ergonomic.json` from this repo and copy it to your Karabiner complex modifications folder:

```bash
mkdir -p ~/.config/karabiner/assets/complex_modifications

cp windows_shortcuts_ms_ergonomic.json \
  ~/.config/karabiner/assets/complex_modifications/
```

Or click this link to import directly (replace `venkatarangan` after publishing):

```
karabiner://karabiner/assets/complex_modifications/import?url=https://raw.githubusercontent.com/venkatarangan/karabiner-mac-to-windows/main/json/windows_shortcuts_ms_ergonomic.json
```

---

### Step 6 — Enable the ruleset

1. Open Karabiner-Elements → **Complex Modifications**
2. Click **Add predefined rule**
3. Find **"Windows Mode for macOS - Microsoft Ergonomic Keyboard"**
4. Click **Enable All**

If you have any other Windows-mode rulesets enabled (e.g. from rux616), disable them first to avoid conflicts.

---

### Step 7 — Turn on "Modify events" for your keyboard AND mouse

> ⚠️ This is the single most commonly missed step. Even with the ruleset enabled, nothing will work until you do this.

**Karabiner-Elements → Devices**

Find your keyboard in the list → flip **"Modify events"** to **ON**.

Then find your mouse in the same list (it appears separately from the keyboard) → flip **"Modify events"** to **ON** for the mouse too. This is required for `Ctrl+Click` multi-select to work.

> ⚠️ These toggles reset to OFF every time you plug in a new device, and reset completely if you delete your Karabiner profile. After any reset, come back here and turn them on again for both devices.

---

### Step 8 — Configure macOS keyboard shortcuts

Karabiner remaps your physical keys before macOS sees them. These macOS shortcuts must be set to match what Karabiner outputs:

| What you want | macOS shortcut to set | Where to set it |
|---|---|---|
| `Ctrl+Space` → Spotlight | Set Spotlight to `Cmd+Space` | System Settings → Keyboard → Keyboard Shortcuts → Spotlight |
| `Win+Space` → Language switch | Set Input Sources to `Ctrl+Space` | System Settings → Keyboard → Keyboard Shortcuts → Input Sources |
| `Win+D` → Show Desktop | Set Show Desktop to `Ctrl+F11` | System Settings → Keyboard → Keyboard Shortcuts → Mission Control |

macOS will warn about a conflict between Spotlight and Input Sources — click OK. There is no real conflict because Karabiner intercepts the keys first and each shortcut only ever receives the signal it expects.

---

## Shortcut reference

### Editing

| Key | Action |
|---|---|
| `Ctrl+C` | Copy |
| `Ctrl+X` | Cut |
| `Ctrl+V` | Paste |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Ctrl+A` | Select all |
| `Ctrl+S` | Save |
| `Ctrl+F` | Find |
| `Ctrl+B` | Bold |
| `Ctrl+I` | Italic |
| `Ctrl+U` | Underline |
| `Ctrl+N` | New |
| `Ctrl+O` | Open |
| `Ctrl+P` | Print |
| `Ctrl+T` | New tab |
| `Ctrl+W` | Close tab |
| `Ctrl+/` | Comment toggle |
| `Win+1` to `Win+9` | Open pinned Dock app 1–9 |

### Cursor & selection

| Key | Action |
|---|---|
| `Ctrl+Left` | Jump word left |
| `Ctrl+Right` | Jump word right |
| `Ctrl+Shift+Left` | Select word left |
| `Ctrl+Shift+Right` | Select word right |
| `Home` | Start of line |
| `End` | End of line |
| `Ctrl+Home` | Start of document |
| `Ctrl+End` | End of document |
| `Shift+Home` | Select to line start |
| `Shift+End` | Select to line end |
| `Ctrl+Shift+Home` | Select to document start |
| `Ctrl+Shift+End` | Select to document end |
| `Ctrl+Backspace` | Delete word left |
| `Ctrl+Delete` | Delete word right |
| `Ctrl+Click` | Multi-select items |

### Window & desktop

| Key | Action |
|---|---|
| `Win` (tap alone) | Spotlight |
| `Ctrl+Space` | Spotlight |
| `Win+E` | Open Finder |
| `Win+L` | Lock screen |
| `Win+D` | Show Desktop (needs Step 8 setup) |
| `Win+Tab` | Mission Control |
| `Win+Space` | Switch input language |
| `Win+R` | Spotlight (Run equivalent) |
| `Win+I` | System Settings |
| `Win+Up` | Maximise window |
| `Alt+Tab` | App switcher |
| `Alt+F4` | Quit app |
| `Ctrl+F4` | Close window / tab |

### Screenshots

| Key | Action |
|---|---|
| `Win+Shift+S` | Region screenshot (snip and drag) |
| `Win+Shift+F` | Screenshot toolbar (full options) |

### Finder

| Key | Action |
|---|---|
| `F2` | Rename file |
| `Delete` or `Backspace` | Move to bin |
| `Ctrl+C` then `Ctrl+V` | Move file to destination (paste = move, like Windows Explorer) |

### Browsers only

| Key | Action |
|---|---|
| `Ctrl+H` | History |
| `Ctrl+L` | Focus address bar |
| `F5` | Reload |

---

## Troubleshooting

**Nothing works at all**
→ Karabiner-Elements → Devices → your keyboard → "Modify events" must be ON. This is the most common cause by far.

**Deleting the Karabiner profile breaks everything after re-import**
→ Deleting the profile resets all "Modify events" toggles to OFF. After re-importing, go to Devices and turn the toggle back on for both your keyboard and your mouse.

**Win key shortcuts fire on the wrong key (Alt instead of Win)**
→ Open EventViewer (menu bar → Karabiner-Elements → Launch EventViewer) and press your Win key. The `name` must show `left_command`. If it shows `left_option`, go to Karabiner-Elements → Simple Modifications → your keyboard → delete any command ↔ option swap. Also check System Settings → Keyboard → Keyboard Shortcuts → Modifier Keys and reset to defaults.

**Win+Space opens Spotlight instead of switching language**
→ Follow Step 8. Spotlight must be set to `Cmd+Space` and Input Sources must be set to `Ctrl+Space`.

**Win+D does nothing**
→ Follow the Show Desktop entry in Step 8. You must assign `Ctrl+F11` to Show Desktop in Mission Control shortcuts.

**Ctrl+Left / Ctrl+Right not jumping words in Terminal**
→ Terminal: Settings → Profiles → Keyboard → check "Use Option as Meta key".
→ iTerm2: Settings → Profiles → Keys → Left Option key → Esc+.

**Ctrl+Click multi-select not working**
→ Karabiner-Elements → Devices → find your mouse (listed separately from the keyboard) → "Modify events" must be ON for the mouse too.

---

## Full setup guide

A complete step-by-step setup guide is included in `docs/mac_windows_keyboard_setup.docx`. It covers Homebrew, Karabiner installation, all permissions, keyboard setup assistant, EventViewer verification, all macOS shortcut assignments, and a smoke test checklist to tick off after each fresh setup.

---

## License

MIT — see [LICENSE](LICENSE)

Copyright (c) 2026 Venkatarangan Thirumalai ([venkatarangan.com](https://venkatarangan.com))

---

## Acknowledgements

Built on top of the foundation laid by [rux616/karabiner-windows-mode](https://github.com/rux616/karabiner-windows-mode). Extended with universal app coverage, Finder-specific shortcuts, screenshots, and a full setup guide.

The ruleset JSON and all documentation in this repo were generated by [Claude Sonnet 4.6](https://www.anthropic.com/claude) based on my instructions and requirements. I did not hand-code any of it — my contribution was describing what I needed, testing the output on my actual setup, and iteratively fine-tuning until it worked correctly. If you find this useful, the credit for translating messy human requirements into working Karabiner JSON goes to Claude.
