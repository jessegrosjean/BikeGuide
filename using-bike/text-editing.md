# Text Editing

![Text Editing](../.gitbook/assets/TextEditing.png)

Text editing should work as you expect. This is a nice feature of Bike. Often outliner applications constrain text editing in various ways. Bike doesn't do that.

### Bike Text Editing

In addition to expected text editing commands Bike also adds a few new ones.

Selection commands:

* Selection > Select Word (`Control-W`)\
  Expand selection to word boundaries.
* Selection > Select Sentence (`Control-S`)\
  Expand selection to sentence boundaries.
* Selection > Select Paragraph (`Shift-Command-L`)\
  Expand selection to paragraph boundaries.
* Selection > Select Branch (`Shift-Command-B`)\
  Expand selection to branch boundaries.
* Selection > Expand Selection (`Option-Command-Up`)\
  Expand the selection up through the different boundary levels.
* Selection > Contract Selection (`Option-Command-Down`)\
  Undo previous Expand Selection command.

Row commands:

* Row > Insert Row (`Command-Return`)\
  This is similar to pressing `Return`. The difference is that it will always just insert a new row. Pressing `Return` will replace the selection with a newline to create the new row.
* Row > Duplicate (`Command-Shift-D`)
* Row > Delete (`Command-Shift-K`)
* Row > Indent (`Control-Command-Right`)
* Row > Outdent (`Control-Command-Left`)
* Row > Move Up (`Control-Command-Up`)
* Row > Move Down (`Control-Command-Down`)

{% hint style="info" %}
Indent and Outdent are important and used frequently. There are multiple keyboard shortcuts to perform these two commands. First you can use `Tab` and `Shift-Tab`as described in [Getting Started](../getting-started.md). Second you can use the above arrow key based shortcuts. Third you can use `Command-]` and `Command-[`.
{% endhint %}

{% hint style="info" %}
In text editing mode, these commands all work on individual rows, unconstrained by the outline structure. This is as you would expect in a text editor, but maybe different than you would expect if you are used to outliners. See [outline editing](outline-editing.md) for outline editing behavior.
{% endhint %}
