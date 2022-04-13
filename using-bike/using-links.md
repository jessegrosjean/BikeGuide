# Using Links

Bike highlights URL and email links in your outlines. For example type any of the following the the link will be highlighted. Click on the link to activate.

* https://www.hogbaysoftware.com
* www.hogbaysoftware.com
* jesse@hogbaysoftware.com

``

{% hint style="info" %}
To activate a link using only your keyboard:

1. Place text caret in the link
2. Choose menu View > Show Context Menu (`Control-M`)
3. Choose "Open Link" from that menu
{% endhint %}

Bike allows you to create links to items in your outline. These links are normal URLs, you can paste them into other macOS apps and they'll still work. For example you can save a Bike link into Apple's "Notes" and when you click that link it will open Bike and select the linked item.

### Creating Links

To create a link:

* Use Edit > Copy Link Shift-Command-C to copy a link to the selected item.
* Drag an item by its triangle handle and then hold down the Control key before releasing the mouse. A link to the dragged item will be inserted into your outline.

The link is composed of two parts. The first part, `dnyzqSRa`, is the root id of the outline. The second part, `Ir`, is the id of the linked item.

```
bike://dnyzqSRa#Ir
```

### Creating Path Links

Use Edit > Copy Path Link to copy a path link to the selected document.

```
bike:///Users/jessegrosjean/Documents/todo.bike#aF
```

Path links use a file path to locate the associated document file instead of using the outline's root id. This makes them more likely to break. If you move or rename the file then the link will stop working. On the other hand path links use simpler technology and may be preferable in some situations. I recommend using the standard id links in most situations.

### What if a link stops working?

Links can "break" for some obvious and non-obvious reasons.

* Create a link to a document and then delete that document. When you activate the link the document won't be found. This is probably not surprising!
* Create a link to an item in a document and then delete that item. When you activate the link the document will be opened, but you'll get a warning that the linked item could not be found.
* Bike uses Spotlight searches to resolve links. It associates the root id with the document file and then searches for that id using Spotlight. This means if something is going wrong with Spotlight then you are links won't work. This is a temporary problem, they will work again once the document id is again indexed with Spotlight.
* This association between a document file and root id is done by setting the `com.apple.metadata:kMDItemIdentifier` extended file attribute each time Bike saves a file. This means if a files extended attributes are somehow lost then links to that file will break until next time the file is opened and saved through Bike.
* If you have two outline documents with the same root id (for example if you duplicate an outline file) then links to that root id will open both documents.
