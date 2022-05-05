# Creating Scripts

UPDATES IN PROGRESS, SCRIPTS MAY NOT WORK UNTIL BIKE (45)

Create scripts to automate Bike and integrate with other apps.

### Bike's Scripting Dictionary

The first step when creating your own scripts is to learn what parts of Bike are scriptable. To do that you need to open Bike's scripting dictionary in the "Script Editor" application that comes your your Mac. The scripting dictionary shows all the commands and objects that you can use when scripting Bike.

#### To open Bike's scripting dictionary:

* &#x20;Drag and drop Bike's application icon onto Script Editor's application icon.
* Or from Script Editor use the menu File > Open Dictionary.

![](<../.gitbook/assets/Screen Shot 2022-05-05 at 12.20.00 PM.png>)

### A quick scripting tour

When scripting Bike you are mainly dealing with `documents`, `windows`, and `rows`.

Documents and windows are common scripting objects common to many applications. Rows are specific to Bike.

Each row represents a row in your outline. Rows have a `name` that is the same as the text shown for the row. Rows also have an `id` and other attributes. Rows can also contain other rows. When you move or delete a row those contained rows are also moved or deleted.

You get rows in a few different ways.

#### There are two important collections of rows in Bike:

First, each document maintains a collection of all rows that are in the document. This collection is a good for when you are looking for a row anywhere in your document, or when you want to process each row in your document.

```
tell front document of application "Bike"
  get row named "Projects" -- returns first row found in your document with name "Projects"
  get row id "todo" -- returns row with id "todo" searching all rows in your document
  get name of every row -- returns a list containing the name of each row
end tell
```

Second, each row also maintains a list of the rows that it directly contains. This collection is good for when you are creating new rows or moving rows around in your outline. It's easier to control exactly where something goes if you add it directly to another row.

```
tell front document of application "Bike"
  get rows of root row -- returns only the top level rows in your outline, not all rows
  move first row of row named "Backlog" to row named "Today" -- move row from "Backlog" to "Today"
end tell
```

### Example scripts

#### Home Script

This script resets your view state to "Home"

```
tell front document of application "Bike"
  set hoisted row to root row
  if exists first row then
    select at first row
  end if
end tell
```

#### Cleanup Script

This script saves the current selected row. Collapses all rows. Then restores your selection, which also expands any rows needed to show the selection.

```
tell front document of application "Bike"
  set saved to selection row
  collapse at root row with completely
  select at saved
end tell
```

#### Today Script

This script create a simple calendar structure in your outline and adds a new line to "today" where you can start taking notes.

```
set yearName to do shell script "date +'%Y'"
set yearId to do shell script "date +'%Y'" & "/00/00"
set monthName to do shell script "date +'%B, %Y'"
set monthId to do shell script "date +'%Y/%m'" & "/00"
set dayName to do shell script "date +'%B %d, %Y'"
set dayId to do shell script "date +'%Y/%m/%d'"

tell application "Bike"
  tell front document
    set y to my getOrMake(yearId, yearName, root row)
    set m to my getOrMake(monthId, monthName, y)
    set d to my getOrMake(dayId, dayName, m)
    select at make row at front of rows of d
  end tell
end tell

to getOrMake(getId, getName, rowContainer)
  tell application "Bike"
    tell container document of rowContainer
      if exists row id getId then
        return row id getId
      else
        tell rowContainer
          return make row at front with properties {id:getId, name:getName}
        end tell
      end if
    end tell
  end tell
end getOrMake

```
