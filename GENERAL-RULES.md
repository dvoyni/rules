# General rules

## Naming

- Identifiers are full words: `shippingAddress`, not `shipAddr`; `configuration`, not `cfg`.
- Type names are singular: `catalog`, not `products`.
- A name says what the thing is, holds or counts, not how it is built or a bare word near it: `nextInvoiceNumber`, `idleWorkers`, `pendingRefundCount`, `priceTable`, not `next`, `idle`, `pending`, `chunks`.
- A function or method name starts with a verb, and names what it acts on when more than one thing could take the same verb: `GrantEditAccess`, not `Editable`. A getter is the exception, named for what it answers: `Total()`, `Line(number)`.

## Work

- Implement exactly what was asked. Anything beyond it is offered, not done.
- A question gets a direct answer. Change code only when asked to.
- A pull request is opened only after the user has approved its text: title and description, shown in full first.
- A pull request stacked on another not yet merged is opened as a draft, and leaves draft once its base merges.
- A ticket's size budget, such as about 200 changed lines, is a recommendation: a feature that needs more may take more, but tickets are kept as granular as the work allows.

## Potential rules

- [1] Work ruled out of an effort's scope is detached from it and left open, not closed: unparent the ticket from the map and keep it for later.
- [1] A new method takes its name from its neighbours in the same API, not from another library's term: `SetFontSize` beside `SetFont` and `SetFontColor`, not `ScaleText` after another toolkit's `tk_scale_text`.
- [2] Commit messages and pull request descriptions carry no Claude attribution: no `Co-Authored-By: Claude` trailer, no "Generated with Claude Code" line.
- [1] A context's glossary is written while its design is mapped, even before its module exists: create `<module>/CONTEXT.md` and its line in `CONTEXT-MAP.md` as soon as a term is settled.
- [2] A new name doesn't reuse a term the project already gives another meaning: the scheduler's list of waiting jobs is a `Backlog`, not a `Queue`, which names the message queue.
- [1] A name for an in-memory runtime structure doesn't borrow database words: the rate limiter's per-client container is a `Bucket`, not a `Table`, `Store` or `Repository`.
- [1] An exclusion covers only the kind of thing the user named; when it lands on a list that mixes kinds, split the list and say back which part goes: "no metrics in v1" for a crash report's fields moved out the CPU and memory figures, not the build number or the stack trace.
- [1] A conversion is no getter: a function that builds one type's value from another's starts with a verb, `makeShippingLabel`, not `shippingLabel`, and neighbours that break a rule set no precedent for new code.
- [1] A name a spec or design sketch gives still follows the naming rules when it is built: a function that only makes a scheduled job is `billing.MakeNightlyJob`, a constructor, not the sketch's `billing.Run`, which runs nothing.
- [1] A precondition a doc comment states is checked in code wherever the code can check it, with a panic or error that names the broken condition, not left to the comment: `Server.AddRoute` says "before Listen", so it panics after `Listen`.
- [1] A doc comment says what an API does in general, and names the caller that motivated it only as an example: `Cache.Freeze` "holds every writer off while the caller reads", and the nightly backup is one such caller, not "Freeze is for the backup".
- [1] A check the user performs by hand is given as numbered steps, each saying what to do and what should appear, and whatever it opens waits long enough to read them first: "1. Open the settings page. 2. Switch the language to French: the File menu should read `Fichier`", with the page opening 20 seconds later.
- [1] A code generator emits only what follows from its input; code that is the same whatever the input is written by hand, in a package of its own, and a check that must cover it, such as a schema hash, reads its source: the varint encoder is `varint`, written by hand, not emitted from templates into the generated `messages` package.
