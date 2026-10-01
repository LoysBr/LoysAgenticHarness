---
name: to-doc-diagram
description: "From a chat or an implementation of a module, generate an explanation diagram in a png/svg/jpg format. File exists next to the module code."
disable-model-invocation: false
---

Called when a system needs a small documentation file to help understand the code or the systems. 

I would like you to represent informations about code classes, architecture, and relations between them : who has a reference of what, where are composition or aggregation, as a diagram you can draw and save as an image (either png, svg, or jpg). To explain complex ideas, make as many diagrams as needed: use-case scenarios, class diagrams, sequence diagrams. The goal is to have a clear understanding of the code and its architecture, and to be able to explain it to others.

To make things visually catchy, you can use colors, shapes, and arrows to represent different types of relationships and interactions between classes. 

There are 4 categories of documentation diagrams you can generate:
- anything aiming to be read by a non programmer and describing the game, located in `doc\gameSystems\` 
- anything aiming to be read by a non programmer and explaining a workflow or a process, located in `doc\howTo\` 
- for programmers, some high level architecture presentation, located in Script folder: `Assets\Scripts\Documentation~\`
- for programmers, something more specific to a module, located in the module folder: `Assets\Scripts\Runtime\TheModuleName\Documentation~\`

For the colors used check the [loysColorblindPalette](../../../docs/loysColorblindPalette.md) file.
