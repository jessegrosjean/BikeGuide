# DOM Context Tutorial

Use the DOM context to display custom UI using HTML/DOM. Currently you can present a sheet over a window or you can add views to the inspector bar.

#### Summary

* [DOM Context API](https://github.com/jessegrosjean/bike-extension-kit/tree/main/api/dom).
* Entry points `dom/*.ts`
* Code runs in web views embedded in Bike’s UI.
* Web views are sandboxed and have no network access.
* These views are loaded dynamically using bike/app context APIs.
* Communicate with app context using `postMessage` and `onMessage`.
* Import dom context API using `import { SYMBOL } from 'bike/dom'`.

## Setup

Turitoral assumes that you have run the `npm run watch` command. Your extension should  automatically build and install when you save changes.

## Create "Archive Done" Sheet

We will to modify the "Archive Done" command to show a sheet with the number of archived rows.

#### Overview

1. Create a dom script at `dom/<scriptname>`.
2. Pass the DOMScriptName to the app context API `window.presentSheet`.
3. Use the returned handle to send the row count to the DOM context for display.

#### Code

Create the DOMScript at `dom/archive-done-sheet.ts`:

```typescript
import { DOMExtensionContext } from "bike/dom";

export async function activate(context: DOMExtensionContext) {
    context.element.textContent = "Loading..."
    context.onmessage = (message) => {
        context.element.textContent = message
    }
}
```

Modify archive done in `app/main.ts` to show the sheet:

```typescript
function archiveDoneCommand(): boolean {
  ...

  bike.frontmostWindow?.presentSheet('archive-done-sheet.js').then((handle) => {
    handle.postMessage(doneRows.length)
  })
  
  return true
}

```

Save, your modified extension should rebuild and install.

Now in Bike perform the archive done command. The sheet should present, telling you how many rows were archived. Press the Escape key to close the sheet.

### Using React

For more complex custom views you might want to use React. Bike bundles and loads a single copy of React into each web view. When you, for example, `import { useState } from 'react'` you will be accessing that shared version of React from your extension.

## Next Steps

Follow the [Style Context Tutorial](style-context-tutorial.md) to create your own outline styles.
