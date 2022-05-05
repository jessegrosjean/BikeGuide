# AppleScript

Bike can be controlled and automated with AppleScript.

UPDATES IN PROGRESS, SCRIPTS MAY NOT WORK UNTIL BIKE (45)

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

This script saves the current selected row, then collapses all rows, then restores that selection, expanding rows as needed to show selection.

```
tell front document of application "Bike"
  set saved to selection row
  collapse at root row with completely
  select at saved
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
