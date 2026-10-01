---
name: to-doc-diagram
description: "From a chat or an implementation of a module, generate an explanation diagram in a png/svg/jpg format. File exists next to the module code."
disable-model-invocation: false
---

Called when a system needs a small documentation file, that you must write next to the scripts, in the project, so if it's to describe the Behaviour system for example, you can write `Assets\Scripts\Runtime\AmbientLife\Documentation~\BehaviourSystemDocumentation.svg` for example. I would like you to represent  informations about code classes, architecture, and relations between them : who has a reference of what, where are composition or aggregation, as a diagram you can draw and save as an image (either png, svg, or jpg). Make as many diagrams as needed: use-case scenarios, class diagrams, sequence diagrams. The goal is to have a clear understanding of the code and its architecture, and to be able to explain it to others.