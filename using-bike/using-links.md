# Using Links

Bike automatically highlights URL and email links in your outlines.

For example type any of the following into Bike and the link will be highlighted. Click on the highlighted link to activate it.

```
https://www.hogbaysoftware.com
www.hogbaysoftware.com
jesse@hogbaysoftware.com
```

When you activate a link it will be opened in the app that has been configured to open that kind of link. For example `http` links will open in your web browser.

{% hint style="info" %}
To activate a link without having to use the mouse:

1. Place text caret somewhere in the link
2. Show context menu with keyboard shortcut `Control-M`
3. Press down arrow to choose "Open Link" from context menu
{% endhint %}

### Bike Links

Bike has its own link type. A link that starts with `bike://` is a bike link. Bike links allow you to link directly to an item in your outline.

Bike links are normal URLS, you can paste them into other macOS apps and they'll still work. For example you can save a Bike link into Apple's "Notes" and when you click that link it will open Bike and select the linked item.

#### To create a Bike link:

Use Edit > Copy Link `Shift-Command-C` to copy a link to the selected item.

Drag an item by its triangle handle and then hold down the `Control` key before releasing the mouse. A link to the dragged item will be inserted into your outline.

This is what a Bike link looks like:

```
bike://dnyzqSRa#Ir
```

Bike links are composed of three parts. The first part, `bike` identifiers the link as a Bike link. The second part, `dnyzqSRa`, is the root id of the linked to outline. The third part, `Ir`, is the id of the linked to item.

A nice feature of these Bike links is that they won't break when you edit your outline or move/rename your outline file.

### Bike Path Links

Bike links also have an alternative form. Path links use a file path to locate the associated document file instead of using the outline's root id. Bike path links look like this:

```
bike:///Users/jessegrosjean/Documents/todo.bike#aF
```

Path links are more likely to break then standard Bike links. If you move or rename the linked to outline then the link will stop working. On the other hand path links use simpler technology and may be preferable in some situations. I generally recommend using the standard id based Bike links.

#### To create a Bike path link:

Use Edit > Copy Path Link to copy a path link to the selected item.

### What if a link stops working?

Links can "break" for some obvious and non-obvious reasons.

* Create a link to a document and then delete that document. When you activate the link the document won't be found. This is probably not surprising!
* Create a link to an item in a document and then delete that item. When you activate the link the document will be opened, but you'll get a warning that the linked item could not be found.
* Bike uses Spotlight searches to resolve links. It associates the root id with the document file and then searches for that id using Spotlight. This means if something is going wrong with Spotlight then you are links won't work. This is a temporary problem, they will work again once the document id is again indexed with Spotlight.
* This association between a document file and root id is done by setting the `com.apple.metadata:kMDItemIdentifier` extended file attribute each time Bike saves a file. This means if a file's extended attributes are somehow lost then links to that file will break until next time the file is opened and saved through Bike.
* If you have two outline documents with the same root id (for example if you duplicate an outline file) then links to that root id will open both documents.
