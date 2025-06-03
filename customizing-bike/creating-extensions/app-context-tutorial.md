# App Context Tutorial

Use the app context to add commands, keybindings, and work with system resources like the clipboard. When creating an extension, the app context is likely where you should start.

#### App Ccontext Summary

* [App Context API](https://github.com/jessegrosjean/bike-extension-kit/tree/main/api/app).
* Entry point `app/main.ts`.
* Code runs in Bike's native app environment.
* Interact with outlines, clipboard, networking, etc.
* Some APIs require appropriate `manifest.json` permissions.
* Import app context API using `import { SYMBOL } from 'bike/app'`.

## Setup

Turitoral assumes that you have run the `npm run watch` command. Your extension should automatically build and install when you save changes.

## Create a new Command

Let's modify the extension that you just created in [Creating Extensions](./) to add a new "Archive Done" command. The command will move all completed items to an "Archive" row.

In `app/main.ts`, add a function to implement the command:

```typescript
function archiveDoneCommand(): boolean {
  console.log("Archive Done!");
  return true;
}
```

In `app/main.ts`, associate that function with a Bike Command:

<pre class="language-typescript"><code class="lang-typescript"><strong>export async function activate(context: AppExtensionContext) {
</strong>  bike.commands.addCommands({
    commands: {
      "extension-name:archive-done": archiveDoneCommand,
    },
  });
}
</code></pre>

Save, and your updated extension should build and then install into Bike.

In Bike, open the Command Pallet (`Command-Shift-P`). You should see your command listed. Select the command, and you should see "Archive Done!" printed in Bike's Logs Explorer window.

Next, let’s add a keybinding to activate this command.

In `app/main.ts`:

```typescript
export async function activate(context: AppExtensionContext) {
  ...
  bike.keybindings.addKeybindings({
    keymap: "block-mode",
    keybindings: {
      "a": "startup:archive-done",
    },
  });
}
```

Save and then switch back to Bike.

In Bike, enter block selection mode (press `Escape`), then type `a`. You should again see "Archive Done!" printed in the Logs Explorer window. Keybindings are used when Bike's outline editor has keyboard focus. They are not used when other UI elements have keyboard focus.

### Implement Archive Done

We will need to:

1. Find done rows.
2. Locate (or create) the Archive row.
3. Move done rows to the Archive row.

In `app/main.ts`, add new Row import and updated archiveDoneCommand:

```typescript
import { AppExtensionContext, Row } from 'bike/app'

...

function archiveDoneCommand(): boolean {
  // Get frontmost editor
  let editor = bike.frontmostOutlineEditor
  if (!editor) return false

  // Get the outline, done rows, and archive row
  let outline = editor.outline
  let donePath = "//@data-done except //@id = archive//*"
  let doneRows = outline.query(donePath).value as Row[]
  let archiveRow = (outline.query("//@id = archive").value as Row[])[0]
  
  // Insert an Archive row if needed and move done rows
  outline.transaction({ animate: "default" }, () => {
    if (!archiveRow) {
      archiveRow = outline.insertRows([{ 
        id: "archive",
        text: "Archive",
      }], outline.root)[0]
    }
    outline.moveRows(doneRows, archiveRow)
  })
    
  return true
}
```

Save, and switch back to Bike.

Mark some rows as done. You can do this by escaping to block selection mode and typing `m d`. Done rows should show with strikethrough text. Once you have some done rows, run the archive command. The done rows should be moved to the Archive row.

## Next Steps

Follow the [DOM Context Tutorial](dom-context-tutorial.md) to show a custom UI sheet when rows are archived.
