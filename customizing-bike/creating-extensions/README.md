# Creating Extensions

Extend and customize Bike with extensions. They introduce new commands, keybindings, views, styles, and more. Sensitive features are safeguarded by a permission system.

## Install Bike Extension Kit

Use the [Bike Extension Kit](https://github.com/jessegrosjean/bike-extension-kit) to create and modify extensions.

The kit requires some setup. The first step to Bike extension development is to download the extension kit and follow the kit's README.md setup instructions. Once you've got it working, the development cycle is fast–save a change to the extension, see results immediately in Bike.

## Extension Development Overview

You've set up the kit and built and installed some existing extensions. Now we'll take a closer look at what an individual extension looks like and what it can do.

Extension structure:

```
extension.bkext
├── manifest.json
├── app (optional)
│   └── main.ts
├── dom (optional)
│   ├── view1.ts
│   └── view2.ts
├── style (optional)
│   └── main.ts
```

Each extension folder has a `manifest.json` which contains the name, permissions, and other metadata. Properties are documented in schemas/manifest.schema.json.

Each subfolder corresponds to a different context where the extension code can run. These contexts are run separately and have different available APIs. An extension might not need to use all contexts. Delete the subfolder for each unused context.

#### app (Application Logic)

* Code runs in Bike's native app environment.
* Interact with outlines, clipboard, networking, etc.
* Some APIs require appropriate `manifest.json` permissions.
* Import app context API using `import { SYMBOL } from 'bike/app'`.

#### DOM (DOM/HTML Views)

* Code runs in web views embedded in Bike’s UI.
* Web views are sandboxed and have no network access.
* These views are loaded dynamically using bike/app context APIs.
* Import bike/dom context API using `import { SYMBOL } from 'bike/dom'`.

#### Style (Outline Editor Styles)

* Used to define custom stylesheets for Bike’s outline editor.
* Use outline paths to match outline elements and apply styles.
* Most extensions will not add styles; delete the src/style folder if unused.
* Import bike/style context API using `import { SYMBOL } from 'bike/style'`.

Most extension development will start in the app context.

The app context gives you direct access to outlines, editors, and system resources. You only need to use the DOM context if you need to display a custom view. The app context and DOM context run separately, but can communicate using `postMessage` and `onmessage`.

The style context is independent, and only needed by extensions that provide outline styles.

## Create Your First Extension

You should have the Bike Extension Kit installed and open it in Visual Studio Code.

We'll be creating a new extension now. Later, we'll add commands, custom views, and styles. The [finished extension](https://github.com/jessegrosjean/bike-extension-kit/tree/main/src/tutorial.bkext) is included with the extension kit. If you get stuck and something doesn't work, check the finished tutorial to see what is different.

### Open Terminal

You need a terminal open to run extension kit commands. You can use the Terminal.app that comes with macOS, or you can use the Terminal app that's built into Visual Studio Code.

### Create Extension

To create a new extension, run the command:

```
npm run new
```

The new extension is created for you in `src`. This command is just creating the folder structure; you could also create a new extension by creating the extension folder and files manually.

### Build & Install Extension

To build your extension, run the command:

```
npm run build
```

New extensions are set to install automatically on each build. If you have Bike running, your extension should now be loaded.

### Rebuild & Install Extension (Watch Mode)

To build and install your extension when you save changes:

```
npm run watch
```

A background process monitors your extension sources for changes. Sometimes, it may need a restart, but generally, it enhances development speed. The rest of the tutorials assume you are in watch mode.

### Debug Extension

There are two important sources for debugging your extension.

#### Logging

To view Bike's log explorer:

* Select the menu Bike > Window > Logs Explorer

Do that now and you should see logs that your extension is installed and activated. You will also see a notice originating from your extension’s call to `console.log` in the activate function.

#### Safari Debugger

For more complex debugging tasks, use Safari's debugger. This allows you to step line by line over your extension code and inspect variables.

To enable Safari's debugger:

* Go to Safari > Settings > Advanced and check "Show features for web developers".
* Go to Safari > Develop > Inspect Apps and Devices to see active contexts.
* Start Bike 2 and it should show in the Safari window from the previous step.

In that window, it will show all of Bike's JavaScript contexts. Click on a context to open it. Then you can set breakpoints, examine variables, etc.

## Next Steps

* [App Context Tutorial](app-context-tutorial.md)
* [DOM Context Tutorial](dom-context-tutorial.md)
* [Style Context Tutorial](style-context-tutorial.md)
