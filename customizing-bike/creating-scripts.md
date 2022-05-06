# Creating Scripts

Create scripts to automate Bike and integrate with other apps.

### Bike's Scripting Dictionary

The first step when creating your own scripts is to learn what parts of Bike are scriptable. To do that you need to open Bike's scripting dictionary in the "Script Editor" application that comes with your Mac. The scripting dictionary shows all the commands and objects that you can use when scripting Bike.

#### To open Bike's scripting dictionary:

* Drag and drop Bike's application icon onto Script Editor's application icon.
* Or from Script Editor use the menu File > Open Dictionary.

![](<../.gitbook/assets/Screen Shot 2022-05-05 at 12.20.00 PM.png>)

### Bike Scripting Overview

When scripting Bike you are dealing with `documents`, `windows`, and `rows`. Documents and windows are common scripting objects with a few Bike extensions–rows are specific to Bike.

Each row represents a row in your outline. Rows have a `name` for accessing the row's text. Rows also have an `id` and other attributes. Rows can contain other rows. When you move or delete a row those contained rows are moved or deleted with it.

You gain access to rows in a few different ways:

1. From properties of a document such as `root row`, `selected row`, `focused row`, etc.
2. From the `rows` collection belonging to each document. This collection contains all rows in the document (except for the root). This collection is a good place to quickly find existing rows.
3. From the `rows` collection belonging to each `row.` This collection contains only the rows that are directly contained by the row (the children). This collection is a good place for making new rows and a target to moving existing rows into.

Generally when scripting you are working with the "model" layer of Bike. You can also script some view state such as `focused row`,  `selected row`, and collapsed state. These view properties are accessible through the document for convenience, but they are really effecting the document's window.&#x20;

Bike allows you to open multiple windows to view a single document. If you have multiple windows viewing a single document than the document's view properties refer to the frontmost of the documents windows.

### Demo Script

It's hard to know where to start.

This is a nonsense script to help you get started. It demonstrates many of Bike's scripting abilities and is a good place to learn how basic things are done. It shows how to make rows, move rows, copy rows, etc.

You'll get the most out of this script by using [Script Debugger](https://latenightsw.com). It's a bit expensive, but you should be able to run the trial for this script. The nice thing about Script Debugger is that it lets you step through the script line by line, so you can see how each command effects the Bike outline separately.

```
tell application "Bike"
  
  -- This script makes a new demo document so that it won't mess up any documents that you have open.
  set demo to make document with properties {name:"Demo"}
  
  tell demo
    -- Bike shows welcome text in new documents, this line deletes that text
    delete every row
    
    -- Create a new "Hello World" row
    set helloWorld to make row with properties {name:"Hello World"}
    
    -- Add some child rows to "Hello World"
    tell helloWorld
      make row with properties {name:"one"}
      make row with properties {name:"two"}
    end tell
    
    -- Insert a new child in into the middle
    tell helloWorld
      make row at (after row named "one") with properties {name:"middle child"}
      
      -- Make a row "at" another row will at it as a child.
      make row at row 2 with properties {name:"middle child child"}
    end tell
    
    -- You can also just make an empty row
    make row
    
    -- Or you can make a row with an id, so its easy to find later, even if it's name has been edited.
    -- If the given id is already in use then the row will still be made, but will get assigned a different id.
    make row with properties {id:"boom", name:"My id is boom"}
    
    -- Change the name of an existing row
    set name of row id "boom" to "You've been renamed"
    
    -- Check to see if a row exists
    if exists row id "boom" then
      log "Yes! row id boom exists"
    end if
    
    -- When "Hello World" moves it brings all containing rows with it.
    move row named "Hello World" to row id "boom"
    move row named "one" to before row named "two"
    
    collapse at row named "Hello World" with completely
    expand at row named "Hello World"
    select at row id "boom"
    
    -- Now just show "Hello World" and contained rows
    set focused row to row named "Hello World"
    
    -- Now just show rows contained by "Hello World"
    set hoisted row to row named "Hello World"
  end tell
  
  -- Make another document
  tell (make document with properties {name:"Move to"})
    -- delete welcome text again
    delete every row
    
    -- Move row from our original document to this new document
    move row named "middle child" of demo to first row
    
    -- Can also just copy rows to new document
    duplicate row named "one" of demo to end of rows
  end tell
  
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

This script saves the current selected row. Collapses all rows. Then restores your selection, which also expands any rows needed to show the selection. Use it to cleanup when you have to many rows expanded, but you still want to keep working where you are.

```
tell front document of application "Bike"
  set saved to selection row
  collapse at root row with completely
  select at saved
end tell
```

#### Today Script

This script create a simple calendar structure in your outline and adds a new line to "today" where you can start taking notes. It's interesting because it uses row `id`'s to track rows. Once the calendar is created you can move it to any place in your outline and the script will keep working.

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
