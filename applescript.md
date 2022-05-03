# AppleScript

Bike can be controlled and automated with AppleScript.

#### Home Script

This script resets your view state to "Home"

```
tell front document of application "Bike"
    set hoisted row to root row
    if exists first child row then
        select at first child row
    end if
end tell
```

#### Cleanup Script

This script saves the current selected row, then collapses all rows, then restores that selection, expanding rows as needed to show selection.

```
tell front document of application "Bike"
    set savedSelection to selection row
    collapse at every child row with completely
    select at savedSelection
end tell
```

#### Today Script

This script adds a new line to the "today" item where you can start taking notes, creating the today item and container dates as needed.

```
set yearName to do shell script "date +'%Y'"
set yearId to do shell script "date +'%Y'" & "/00/00"
set monthName to do shell script "date +'%B, %Y'"
set monthId to do shell script "date +'%Y/%m'" & "/00"
set dayName to do shell script "date +'%B %d, %Y'"
set dayId to do shell script "date +'%Y/%m/%d'"

tell application "Bike"
    set doc to front document
    tell my makePath(doc, {{yearId, yearName}, {monthId, monthName}, {dayId, dayName}})
        set inserted to make child row at front
        tell doc to select at inserted
    end tell
end tell

to makePath(doc, pathItems)
    tell application "Bike"
        set current to doc
        repeat with segment in pathItems
            set segId to item 1 of segment
            set segName to item 2 of segment
            if exists row id segId of doc then
                set current to row id segId of doc
            else
                tell current
                    set current to make child row at front with properties {id:segId, name:segName}
                end tell
            end if
        end repeat
        return current
    end tell
end makePath
```
