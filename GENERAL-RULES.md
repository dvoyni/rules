# General rules

## Naming

- Identifiers are full words: `pinnedToMainThread`, not `pinned`; `configurator`, not `cfg`.
- Type names are singular: `dispatcher`, not `eventHandlers`.
- A name says what the thing is, holds or counts, not how it is built or a bare word near it: `nextFreshIndex`, `freeEntities`, `reservationCount`, `indexTable`, not `fresh`, `free`, `reserved`, `pages`.
- A function or method name starts with a verb, and names what it acts on when more than one thing could take the same verb: `ElevateFileAccess`, not `Writable`. A getter is the exception, named for what it answers: `Size()`, `Entry(window)`.

## Work

- Implement exactly what was asked. Anything beyond it is offered, not done.
- A question gets a direct answer. Change code only when asked to.
- A pull request is opened only after the user has approved its text: title and description, shown in full first.
- A pull request stacked on another not yet merged is opened as a draft, and leaves draft once its base merges.
- A ticket's size budget, such as about 200 changed lines, is a recommendation: a feature that needs more may take more, but tickets are kept as granular as the work allows.

## Potential rules

- Work ruled out of an effort's scope is detached from it and left open, not closed: unparent the ticket from the map and keep it for later. (Taught when the macOS Objective-C ticket was closed as out of scope on the gogpu loop map.)
- A new method takes its name from its neighbours in the same API, not from another library's term: `SetCursorPosition` beside `SetCursor` and `SetCursorMode`, not `WarpCursor` after SDL's `SDL_WarpMouseInWindow`. (Taught when gogpu's cursor move was first named `WarpCursor`.)
- Commit messages and pull request descriptions carry no Claude attribution: no `Co-Authored-By: Claude` trailer, no "Generated with Claude Code" line. (Taught when the gogpu cursor-warp PR draft ended with "🤖 Generated with Claude Code".)
- A context's glossary is written while its design is mapped, even before its module exists: create `<module>/CONTEXT.md` and its line in `CONTEXT-MAP.md` as soon as a term is settled. (Taught when the system filesystem terms were left in a ticket comment because the `system` module had no directory yet.)
- A new name doesn't reuse a term the project already gives another meaning: the generator's set of marked structs is `Catalog`, not `Model`, which names the game state; the World's per-entity-type container is a `Collection`, not a `Storage`, which names the Storage Service. (Taught when the generator's `Model` was renamed `Catalog` in gtb.)
- A name for an in-memory runtime structure doesn't borrow database words: the World's per-entity-type container is a `Collection`, not a `Table`, `Store` or `Repository`. (Taught when `Table` was proposed for it in gtb.)
- An exclusion covers only the kind of thing the user named; when it lands on a list that mixes kinds, split the list and say back which part goes: "no metrics in M1" for a panic entry's diagnostics moved out the process and host figures, not the build or the World's own counts. (Taught when every panic diagnostic was deferred to M2 in gtb.)
- A name a spec or design sketch gives still follows the naming rules when it is built: a function that only makes a tick list entry is `world.MakeSystemStep`, a constructor, not the spec's `world.Update`, which updates nothing. (Taught when gtb's tick list entry was first built as the spec's `world.Update`.)
- A precondition a doc comment states is checked in code wherever the code can check it, with a panic or error that names the broken condition, not left to the comment: gtb's `World.SetResource` says "before Start", so it panics after `Start`. (Taught when `SetResource` documented "before Start" and checked nothing.)
