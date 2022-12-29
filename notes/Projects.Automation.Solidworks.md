---
id: iiarz11zzpdnj4y6axlrjra
title: Solidworks
desc: ''
updated: 1665953453053
created: 1665947610322
tags: Project
filesaveloc: ''
priority: null
startDate: 'TODO: date to start'
launchDate: 'TODO: when project ends'
---


## Title
Solidworks

## Description
Light Solidworks automation system to enable quick generation and manipulation of parts/assemblies/drawings. 

Currently working with it just as VBA macros, so limited on I/O. Need to determine modular system to enable flexibiltiy and speed of development. 

Modular system to enable VBA:
- Requires Excel Worksheet in order to call/run VBA macro
    - Can run hidden in background
    - Can call from python via XLwings?
    - Can call from AHK
- standard template folder structure generated/copied from template on initialization:
    - CSV files:
        - file properties (name, description, material props, etc.)
        - Features (sketches, features, etc.)
        - ASM properties (included parts, mates between features)
- Store location to specific file in file properties?
    - macro generates path to specific file from a standard root location
    - Organized by project, matching PDM folder structure. 

Functions:
- OnOpen, OnTrigger:
    - Update SW file from CSV files
- OnSave:
    - Update CSV files from SW file

## Goal
<!-- What are you trying to accomplish -->
- Generate PRT/ASM/DRW
- Insert PRT/ASM into PRT/ASM
- Create sketches with given dimensions
- Create Features with given dimensions


## Context
<!-- Related Projects - Ideally build this into an automated "what's this building on/leading to" filler spot -->

## Success Criteria
<!-- milestones for this project -->

## Sub Projects
<!-- For larger projects, list out sub projects related-->

## Log
<!-- For longer projects, keep a rough log of major events-->

## Outputs
<!-- any outputs that were generated from this project. eg. slides, videos, etc-->

<!-- Everything below this line is work needed to achieve the stated goal-->

## Tasks
<!-- use this space to track current tasks. alternatively, you can also link to your daily journal note -->

## Notes
<!-- use this space for arbitrary notes -->

## Ideas
<!-- relevant thoughts, ideas, or resources -->

