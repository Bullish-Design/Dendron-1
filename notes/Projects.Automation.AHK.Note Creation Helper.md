---
id: vtdphaa71bshn2nc8cpc7k9
title: Note Creation Helper
desc: ''
updated: 1663910834970
created: 1663792988206
---

# Automation Name
Note Creation Helper

## Functionality
Extend functionality of the included Dendron special notes (https://wiki.dendron.so/notes/5c213aa6-e4ba-49e8-85c5-1bdcb33ce202/) to better fit the product development workflow. 

## Sub-Functions

### Call Keyboard Shortcut - Done
    Calls the shortcut for New Task Note, New Project Note

    Reasoning for the "keyboard shortcut to keyboard shortcut" is to have a hook that can be called from other programs/situations

    For now, just have a shortcut that ensures VScode is active window and then calls the note/task shortcut. 

    NOTE: Make a global macro to focus the vscode window and call the command pallate. 

    NOTE: VScode spell checker?

    - Inputs: Keyboard Shortcuts
    - Outputs: Keyboard Shortcuts

#### Recognize shortcut list
    Modular aspect of the above. "Database" of different keyboard shortcut inputs and outputs. 

    Will allow modularity in the future for interaction from other programs/scenarios.

### Grab target file name - Done 
    Current file name + ".task" or ".note" 

    AHK - current window name? Parsed to find and remove the ".md"

    NOTE: AHK activity logger to keep running log of active VS code windows - this function can provide you with last 3-5 windows to pick which one to use. 



### Remove ".md", return remainder - Done
    Useful filename modification snippet for ahk

    - Inputs: File suffix
    - Outputs: Everything in the file name before the suffix

### Search full file list for index count - check if exists
    Counter = 0

    Counter=+1

    Check if Filename.Task.(Counter) exists

    If yes, loop
    If no, return filename

    Create filename

### Generate new file
    Ensure text in "create new note" bar is highlighted

    Paste in new note name
### Provide text input for task or note description
    Eventually - have this be a VScode input bar at the top of the screen

    For now - ensure the text cursor goes to the right location in the page to type under a header that can be brought up a level 

### Add text input to new file
    Eventual Goal - Not currently needed (see above)

NOTE: Figure out cleaner formatting for headers - indent each level in the web exported version?

NOTE: Time is always lost when fiddling around with random code changes trying to hunt for bugs. Better off switching to something else and coming back with fresh eyes. 

NOTE: Make Dendron Logging Function