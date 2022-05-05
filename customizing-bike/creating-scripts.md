# Creating Scripts

UPDATES IN PROGRESS, SCRIPTS MAY NOT WORK UNTIL BIKE (45)

Create scripts to automate Bike and integrate with other apps.

### Bike's Scripting Dictionary

The first step when creating your own scripts is to learn what parts of Bike are scriptable. To do that you need to open Bike's scripting dictionary in the "Script Editor" application that comes your your Mac. The scripting dictionary shows all the commands and objects that you can use when scripting Bike.

#### To open Bike's scripting dictionary:

* &#x20;Drag and drop Bike's application icon onto Script Editor's application icon.
* Or from Script Editor use the menu File > Open Dictionary.

![](<../.gitbook/assets/Screen Shot 2022-05-05 at 12.20.00 PM.png>)

### Bike Scripting Overview

When scripting Bike you are dealing with `documents`, `windows`, and `rows`. Documents and windows are common scripting objects, rows are specific to Bike.

Each row represents a row in your outline. Rows have a `name` for accessing the row's text. Rows also have an `id` and other attributes. Rows can contain other rows. When you move or delete a row those contained rows are moved or deleted with it.

You access rows in a few different ways:

1. From properties of a document such as `root row`, `selected row`, `focused row`, etc.
2. From the `rows` collection belonging to each document. This collection contains all rows in the document. This collection is a good place to quickly find existing rows.
3. From the `rows` collection belonging to each `row.` This collection contains only the rows that are directly contained by the  row (children). This collection is a good place for making new rows and a target to moving existing rows into.

Generally when scripting you are working with the "model" layer of Bike. You can also script some view state such as `focused row`,  `selected row`, and collapsed state.

These view properties are accessible through the document for convenience, but Bike allows you to open multiple windows to view a single document. If you have multiple windows viewing a single document than those view properties refer to the `frontmost window` of the documents windows.

### Demo Script

It's hard to know where to start.

This is a nonsense script to help you get started. It demonstrates many of Bike's scripting abilities and is a good place to see how basic things are done. It shows how to make rows, move rows, copy rows, etc.

You'll get the most out of this script by using [Script Debugger](https://latenightsw.com). It's a bit expensive, but you should be able to run the trial on this script. The nice thing about Script Debugger is that it lets you step through the script line by line, so you can see how each command effects the Bike outline separately.

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
