# Creating Extensions

Extend and customize Bike with extensions. They introduce new commands, keybindings, views, styles, and more. Sensitive features are safeguarded by a permission system.

This section of the user guide provides an overview and tutorial. For the most up-to-date and detailed API documentation, see [bike-extension-kit/api](https://github.com/jessegrosjean/bike-extension-kit/tree/main/api). If you prefer not to follow the step-by-step instructions, I’ve also created screencasts that cover the same information.

* [Setup & Build](https://vimeo.com/1089520938)
* [Creating Extensions](https://vimeo.com/1089816472)
* [App Context Extensions](https://vimeo.com/1089829088)
* [DOM Context Extensions](https://vimeo.com/1089831661)
* [Style Context Extensions](https://vimeo.com/1090188813)

## Install Bike Extension Kit

Use the [Bike Extension Kit](https://github.com/jessegrosjean/bike-extension-kit) to create and modify extensions.

The extension kit requires setup. First, download the kit and follow the kit's README.md setup instructions. Once you've got it working, the development cycle is fast–save a change to the extension, then Bike reloads the extension immediately.

## Extension Development Overview

You've set up the kit and built and installed some existing extensions. Now we'll take a closer look at what an individual extension looks like and what it can do.

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
├── theme (optional)
│   ├── theme1.bktheme
│   └── theme2.bktheme
```

Each extension has a `manifest.json` file which contains the name, permissions, and other metadata. Properties are documented in the extension kit; schemas/manifest.schema.json.

Each subfolder corresponds to a different context where the extension code can run. These contexts are run separately and have different available APIs. An extension might not need to use all contexts, and you can safely delete the folder of each unused context.

#### app context (Application Logic)

* Code runs in Bike's native app environment.
* Interact with outlines, clipboard, networking, etc.
* Some APIs require appropriate `manifest.json` permissions.
* Import app context API using `import { SYMBOL } from 'bike/app'`.

#### DOM context (DOM/HTML Views)

* Code runs in web views embedded in Bike’s UI.
* Web views are sandboxed and have no network access.
* These views are loaded dynamically using app context APIs.
* Import DOM context API using `import { SYMBOL } from 'bike/dom'`.

#### Style context (Outline Editor Styles)

* Used to define custom stylesheets for Bike’s outline editor.
* Use outline paths to match outline elements and apply styles.
* Most extensions will not add styles; delete the style folder if unused.
* Import style context API using `import { SYMBOL } from 'bike/style'`.

The app context and DOM context can communicate using the `postMessage` and `onmessage` methods. The common pattern involves performing work in the app context, such as querying the outline or making network requests, and then sending the results to the DOM context for display.

There is also a `theme` folder. Themes are configuration files used by the style context. Any themes included with an extension will show up in Bike's themes menus when the extension is installed. Themes can also be installed in Bike independent of an extension.

## Create Your First Extension

You should have the Bike Extension Kit installed and open it in Visual Studio Code.

We'll be creating a new extension now. Later, we'll add commands, custom views, and styles. The [finished extension](https://github.com/jessegrosjean/bike-extension-kit/tree/main/src/tutorial.bkext) is included with the extension kit. If you get stuck and something doesn't work, check the finished tutorial to see where my instructions went wrong.

### Open Terminal

You need a terminal open to run extension kit commands. You can use the Terminal.app that comes with macOS, or you can use the Terminal that's built into Visual Studio Code.

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

A background process monitors your extension for changes. The rest of the tutorials assume you are in watch mode, so as soon as you save changes, the results are loaded into Bike.

### Debug Extension

There are two important sources for debugging your extension.

#### Logging

To view Bike's Log Explorer:

* Choose the menu Bike > Window > Logs Explorer

Do that now and you should see in the logs that your extension is installed and activated. You will also see a notice originating from your extension’s call to `console.log` in the activate function.

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
