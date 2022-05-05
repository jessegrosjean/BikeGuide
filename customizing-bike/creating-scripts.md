# Creating Scripts

Create scripts to automate Bike and integrate with other apps.

### Getting Started

1. Open the "Script Editor" application that comes with your Mac.
2. Paste the following script into a new editor window.
3. Make sure that the scripting language is set to "AppleScript".
4. Press the "Play" button to run the script. The script will create a new document and add a "Hello" row that contains a "World" row.

```
tell application "Bike"
  tell (make document with properties {name:"Testing!"})
    delete every row
    tell (make row with properties {name:"Hello"})
      make row with properties {name:"World"}
    end tell
  end tell
end tellScripting Overview
```

The things you can script are documented in Bike's Scripting Dictionary.

To open Bike's scripting dictionary:

* &#x20;Drag and drop Bike's application icon onto Script Editor's application icon.
* Or from Script Editor use the menu File > Open Dictionary.

The scripting dictionary (as shown below) defines all the Bike specific commands and objects that you can use. Bike's scripting dictionary is pretty nice and simple!:thumbsup::thumbsup::thumbsup:

![](<../.gitbook/assets/Screen Shot 2022-05-05 at 12.20.00 PM.png>)
