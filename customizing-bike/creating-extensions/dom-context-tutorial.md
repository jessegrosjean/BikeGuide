# DOM Context Tutorial

Use the DOM context to display custom UI using HTML and DOM.

Currently, you can present a custom sheet over a window or add custom views to the inspector bar. Note, your extension might not need to use the DOM context. Instead, you can use the app context to present alerts and add items to the sidebar, just not fully custom UI.

#### App Context Summary

* [DOM Context API](https://github.com/jessegrosjean/bike-extension-kit/tree/main/api/dom).
* Entry points: `dom/*.ts(x)`
* Code runs in web views embedded in Bike’s UI.
* Web views are sandboxed and have no network access.
* These views are loaded dynamically using app context APIs.
* Communicate with the originating app context using `postMessage` and `onMessage`.
* Import DOM context API using `import { SYMBOL } from 'bike/dom'`.

## Setup

Turitoral assumes that you have run the `npm run watch` command. Your extension should automatically build and install when you save changes.

## Create "Archive Done" Sheet

We will modify the "Archive Done" command to show a sheet with the number of archived rows.

#### Overview

1. Create a DOM script at `dom/<scriptname>`.
2. Pass the DOMScriptName to the app context API `window.presentSheet`.
3. Use the returned handle to send the row count to the DOM context for display.

#### Code

Create the DOM script at `dom/archive-done-sheet.ts`:

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
    handle.postMessage(doneRows.length);
  })
  
  return true;
}

```

Save, your modified extension should rebuild and install.

Now in Bike, perform the archive done command. The sheet should present, telling you how many rows were archived. Press the Escape key to close the sheet. In code, you can close the sheet by calling `dispose()` on the returned handle.

### Using React

For more complex custom views, you might want to use React.

Bike bundles and loads a single copy of React into each web view. The extension kit is set up to use that bundled version when you import react into your DOM script. You may also use the `.tsx` file extension and syntax in your DOM script.

## Next Steps

Follow the [Style Context Tutorial](style-context-tutorial.md) to create your own outline styles.
