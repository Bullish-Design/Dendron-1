---
id: 6n3id60jlhw9j4bqpvxxzem
title: DendronCodeTemplate
desc: ''
updated: 1664313721503
created: 1664225618725
tags: Code
version: ''
filesaveloc: 'G:\My Drive\Google Drive - Projects\Code\Python'
language: '.py'
priority: null
---

# Overview
TODO Create a python script to transform a "Code" Dendron template into properly formatted code.  


# Sub-Functions
<!-- Overview of any function interplay or broad overviews -->


## Open File
Description: Import a markdown file and store it line by line for later use. 
    Input: Folder Location, Filename
    Output: File Contents line by line in array

```python

def read_Dendron_MD_File(mdFile):
    f = open(mdFile)
    allLines = f.readlines()
    returnLines = []
    for eachLine in allLines:
        returnLines.append(eachLine) # previously eachLine.strip(), but that removes indentation
    return returnLines

```

## Code Block/Comment switcheroo
Description: Recognize dendron code blocks, turn those into code - everything else turn into comments
    Input: Array from above, Block comment character start, block comment character end, 
    Functionality: Insert comment character on line 1, Find code block, replace with comment end. Repeat, swapping code open for comment block end and code close to comment block start. 
    Output: Array of lines formatted for functioning as code.

```python

def convertMDfiletoCode(mdFile, blockCommentStart, blockCommentEnd):
    mdFileAsArray = read_Dendron_MD_File(mdFile)
    mdFileAsArray.insert(0,blockCommentStart)

    for i in range(len(mdFileAsArray)):
        if mdFileAsArray[i].startswith('```python'):
            mdFileAsArray[i] = blockCommentEnd
        if mdFileAsArray[i].startswith('```'):
            mdFileAsArray[i] = blockCommentStart
    mdFileAsArray.insert(-1, blockCommentEnd)
    return mdFileAsArray

```

## Save output
Use template defined "save" location to save output file as copy

```python

def saveArrayToFile(array, fileName):
    with open(fileName, 'w') as f:
        for line in array:
            f.write("".join(line))


```

## Recognize frontmatter and extract data to array
Get code language, file save location, save as name, etc.

```python

def grabFrontmatter(mdFile):
    

```


## Link to Output
Provide link to output file to open in VScode or in File Explorer

```python



```


## Language adaptability
Immediate need for AHK and Python, will need others later

Function to associate filetype in header to block comment start/end

## Block Import
Also copy code blocks into a common code snippet file for each language? 
- And have easy reference to/from this library to make it easier to use/reuse code functions










