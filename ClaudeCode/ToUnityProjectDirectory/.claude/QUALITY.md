# Project Quality Evaluation

This is how this project will be evaluated: it has to respect those quality criterias, and the role of QA is also to check those.

## Expandability

The cost to add the next game element from an existing feature. For example, if the game has 3 NPC behaviors, how much does it cost to add a 4th? The architecture of the feature should be ready to easily add more in the future.

## Configurability

Whether content lives in the editor as data, or in code. How the systems compose. Is it easily configurable by a non programmer in the Editor? If some data is stored in a remote database, relevant settings should live there.

## Edge cases

What happens when things overlap, repeat, get interrupted, or get skipped.
Note: Depending on the project, user may want a different level of testing. For example, should we test only in game (during Play), or also Editor features? Should Game be stable even when settings asset change on disc or when Scene editable objects are edited at runtime?

## Architecture and data modeling

Systems code structure should follow a clear logic. Which classes can be grouped in bigger "Modules"? What can be dependant of what? What should know or not know about what? A good base to apply are "SOLID" principles ([Wikipedia SOLID principles page](https://en.wikipedia.org/wiki/SOLID)).

## Code quality, simplicity and organization

Clean, readable, doing more with less.

## Debuggability

Whether it is clear what is happening in the game and why, without reading the source.

## Art pipeline fluency

Deliberate import configuration, and the ability to diagnose a broken asset rather than work around it. Should be detailed in `docs/howTo/assetsPipeline.md`.
