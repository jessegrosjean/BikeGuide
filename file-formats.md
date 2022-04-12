# File Formats

Bike supports three file formats: Bike, OPML, and Plain Text.

* When you first save a document you will be presented with a "Save As" file dialogue. At the bottom you will see a "File Format" popup button for choosing file format.
* To change an existing document's file format choose the menu `File > Duplicate (Command-Shift-S)`. The document will be duplicated and you will be presented with the "Save As" dialogue where you can choose a new format.

### File Format Options

`.bike`: This is Bike's native format and the one I would recommend using. It supports all Bike features and it's also a HTML document so you can view Bike files in your web browser.

`.opml`: This is a standard format for saving outlines. Use `.opml` if you wish to edit your outline with Bike and another OPML compatible outliner.

`.txt`: Bike can also open/save any plain text file as an outline. The outline structure is determined by the leading tab indentation. Text files don't offer any good place to store metadata (such as item ids). For this reason some features (such as links to items) will break when you close and then reopen a `.txt` based file.
