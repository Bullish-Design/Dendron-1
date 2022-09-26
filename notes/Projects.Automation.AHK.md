---
id: egjb9yt0ftsjkhp8nzru75w
title: AHK
desc: ''
updated: 1664224858554
created: 1663736355962
---

# Dendron macros:

## Note creation helper
Have a more streamlined interface for creating new notes, instead of just the CTRL+L provided by dendron. Wanting this to just add another incremental note to the collection at current location, though specifiying locations might be good functionality to have. Ideally, the flow would go like this:

    1. Hit keyboard shortcut
    2. AHK figures out the current file name tree structure, and parses how many notes currently exist there, if any. 
    3. Increments note counter by one, passes filename back to VS Code
    4. Meanwhile, another little VS Code text insert box pops up asking you what you'd like the task to be named. This goes into the front matter, and becomes a header before the note inserts whatever other schema template is desired. 
    5. Generate the note. 

The end goal would be parsing all the notes to be able to pull them into an index in the main project note location, and being able to link back and forth. 

    Functionality pt 2: That means it should also auto-generate the necessary links to go up and down the tree as needed without needing to CTRL-TAB
 
