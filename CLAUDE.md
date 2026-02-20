# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

BikeGuide is the Markdown-based user documentation for the Bike outliner application. It is hosted via GitBook and serves as the official user guide for Bike 2.

## Repository Structure

```
BikeGuide/
├── README.md                    # Landing page (Bike 2 Preview)
├── SUMMARY.md                   # GitBook table of contents (defines navigation)
├── using-bike/                  # End-user documentation
│   ├── outline-editing.md
│   ├── using-selection.md
│   ├── using-scripts.md
│   ├── using-extensions.md
│   ├── using-outline-paths.md
│   └── ...
├── customizing-bike/            # Developer/power-user documentation
│   ├── creating-keybindings.md  # Custom keyboard shortcuts
│   ├── creating-scripts.md      # AppleScript examples
│   ├── creating-themes.md       # Theme creation
│   ├── creating-shortcuts.md    # macOS Shortcuts integration
│   └── creating-extensions/     # Extension development tutorials
│       ├── app-context-tutorial.md
│       ├── dom-context-tutorial.md
│       └── style-context-tutorial.md
└── .gitbook/assets/             # Screenshots and images
```

## Editing Guidelines

**Navigation**: The `SUMMARY.md` file defines the GitBook table of contents. When adding new pages, update this file to include them in the navigation.

**Images**: Store screenshots and images in `.gitbook/assets/`. Reference them using relative paths like `../.gitbook/assets/image.png`.

**Cross-References**: Use relative markdown links to reference other pages within the guide.

**GitBook Hints**: The documentation uses GitBook-specific syntax for callouts:
```markdown
{% hint style="info" %}
Helpful tip here
{% endhint %}
```

## Cross-Repository Sync

When Bike app features or APIs change, this documentation must be updated:

- **Extension API changes** (`bike-extension-kit/api/`): Update `customizing-bike/creating-extensions/` tutorials
- **New app features** (`Bike/`): Update relevant `using-bike/` pages
- **Theme/style changes**: Update `customizing-bike/creating-themes.md`
- **Keybinding changes** (`Bike/OutlineEditor/.../Keymaps/`): Update `customizing-bike/creating-keybindings.md`

The extension API documentation in `customizing-bike/creating-extensions/` references the [bike-extension-kit](https://github.com/jessegrosjean/bike-extension-kit) repository for detailed API specifications.
