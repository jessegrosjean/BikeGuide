# Creating Scripts

Create scripts to automate Bike and integrate with other apps. If you are just interested to run a script that someone else has written please see the [Using Scripts](../using-bike/using-scripts.md) section.

### Overview

When scripting Bike you are dealing with `documents`, `windows`, and `rows`. Documents and windows are common scripting objects with a few Bike extensions–rows are specific to Bike.

Each row represents a row in your outline. Rows have a `name` for accessing the row's text. Rows also have an `id` and other attributes. Rows can contain other rows. When you move or delete a row those contained rows are moved or deleted with it.

You gain access to rows in a few ways:

1. From properties of a document such as `root row`, `selected row`, `focused row`.
2. From the `rows` collection belonging to each document. This collection contains all rows in the document (except for the root). This collection is a good place to quickly find existing rows.
3. From the `rows` collection belonging to each `row.` This collection contains only the rows that are directly contained by the row (the children). This collection is a good place for making new rows and to use as a target to moving existing rows into.

### Dictionary

Use Bike's scripting dictionary to learn what parts of Bike are scriptable.

#### To open Bike's scripting dictionary:

* Drag and drop Bike onto Script Editor's application icon.
* Or from Script Editor use File > Open Dictionary and choose Bike's dictionary.

![](<../.gitbook/assets/Screen Shot 2022-05-05 at 12.20.00 PM.png>)

### Getting Started

Here's the official starting point for learning AppleScript:

* [Introduction to AppleScript Language Guide](https://developer.apple.com/library/archive/documentation/AppleScript/Conceptual/AppleScriptLangGuide/introduction/ASLR\_intro.html)

The starting point for lesser people, such as myself, is to find example scripts and then randomly change them until they do what you want. I've included some for you below. You can also find scripts in the Bike support form and ask scripting questions.

* [ ](https://support.hogbaysoftware.com/c/bike/22)[Bike Support Forum – Extensions Wiki](https://support.hogbaysoftware.com/t/bike-extensions-wiki/4810)

### Example scripts

You'll get the most out of these scripts by using [Script Debugger](https://latenightsw.com) instead of Script Editor that comes with your Mac. Among other things Script Debugger allows you to step through the script line by line so you can see the effect of each command on you document.

#### Kitchen Sink

This is a nonsense script that demonstrates many of Bike's scripting abilities. It's a good place to learn how basic things are done like making and moving rows.

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
      make row with properties {name:"rich text"}
    end tell
    
    tell row named "rich text"
      set bold of first word of text content to true
      set italic of second word of text content to true
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
    
    tell row id "boom"
      -- Read/Write row level attributes
      exists (attribute named "test") -- false
      make attribute with properties {name:"test", value:"value"}
      value of attribute named "test" -- value
      set value of attribute named "test" to "new value"
      value of attribute named "test" -- new value
      delete attribute named "test"
    end tell
    
    -- When "Hello World" moves it brings all containing rows with it.
    move row named "Hello World" to row id "boom"
    move row named "one" to before row named "two"
    
    collapse row named "Hello World" with all
    expand row named "Hello World"
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
  collapse root row with all
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
  using terms from application "Bike"
    tell container document of rowContainer
      if exists row id getId then
        return row id getId
      else
        tell rowContainer
          return make row at front with properties {id:getId, name:getName}
        end tell
      end if
    end tell
  end using terms from
end getOrMake
```
