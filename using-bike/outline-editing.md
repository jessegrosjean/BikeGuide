# Outline Editing

![Outline Editing](../.gitbook/assets/outline-editing.png)

In addition to [Text Editing](text-editing.md) Bike supports outline editing mode. When in outline editing mode rows are fully selected and shown in a different color.

#### To enter and exit outline editing mode

* Press the `Escape` key to enter outline editing
* Press the `Escape` key again to exit outline editing mode
* Or Click to make a selection with your mouse to exit outline editing mode
* Or press `Return` to create a new item and exit outline editing mode.
* ...lots of ways out, use the `Escape` to enter!

### Outline Editing Commands

In Outline Editing mode the arrow keys have different behavior:

* `Left` Key\
  Collapse expanded rows
* `Right` Key\
  Expand collapsed rows
* `Up` Key\
  Move up by row
* `Down` Key\
  Move down by row

The row commands described in the [Text Editing](text-editing.md) section work differently too. They work on a row and all contained rows, not just individual rows. Movements are also restricted to the outline structure.

* Row > Duplicate (`Command-Shift-D`)
* Row > Delete (`Command-Shift-K`)
* Row > Indent (`Control-Command-Right`)
* Row > Outdent (`Control-Command-Left`)
* Row > Move Up (`Control-Command-Up`)
* Row > Move Down (`Control-Command-Down`)

Give it a try. Create a small outline and then try the above commands. First in text editing mode, second in outline editing mode. You'll soon see and understand the differences. Outline Editing mode can save a lot of time.

### More Outline Editing Commands

Some command work the same in both modes:

* Row > Insert Row (`Command-Return`)\
  Insert a new row
* Row > Group (`Option-Command-G`)\
  Inserts a new row above the selection. And then moves the selection into that new row.
