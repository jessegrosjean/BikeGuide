# Creating Keybindings

Customize Bike's keybindings through the Keybindings settings panel.

### Settings Panel

Open **Settings > Keybindings** to view all commands and their current keybindings. Select a command and press Return (or double-click the keybinding cell) to edit it. Press Delete to clear a custom keybinding. User-customized bindings are highlighted in accent color.

### Key Sequence Format

A key sequence is one or more keys separated by spaces. Each key is a combination of modifiers and a key name joined by hyphens. The format is case-insensitive.

**Modifiers:**

| Modifier | Alias |
| -------- | ----- |
| Command  | cmd   |
| Control  | ctrl  |
| Option   | opt   |
| Shift    |       |

**Named keys:**

Return, Tab, Space, Delete, Escape, ForwardDelete, Home, End, PageUp, PageDown, LeftArrow (alias: left), RightArrow (alias: right), UpArrow (alias: up), DownArrow (alias: down), F1–F20, CapsLock, Function, Help

Any single typed character (a–z, 0–9, punctuation) is also a valid key.

**Examples:**

| Key Sequence    | Description                            |
| --------------- | -------------------------------------- |
| `cmd-s`         | Command-S                              |
| `ctrl-shift-a`  | Control-Shift-A                        |
| `space`         | Space key (no modifiers)               |
| `cmd-k cmd-c`   | Chord: Command-K followed by Command-C |
| `ctrl-x ctrl-s` | Chord: Control-X followed by Control-S |
| `m d`           | Chord: M followed by D (no modifiers)  |

### Full API Reference

For the complete keybindings API including programmatic access from extensions, see the [keybindings type definitions](https://github.com/jessegrosjean/bike-extension-kit/blob/main/api/app/keybindings.d.ts).
