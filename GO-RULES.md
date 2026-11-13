# Go rules

## Naming

Lifecycle verbs come in fixed pairs. Pick the pair by what the code does, and keep each verb to its one meaning:

| Pair | Meaning |
|---|---|
| `new*` / `make*` | Constructors. `new*` returns a pointer, `make*` returns a value, as Go's `new` and `make` do. |
| `init` / `destroy` | Bring up and tear down something that already exists. A failed `init` destroys what it already initialized, in reverse order. `Start` / `Stop` run between them. |
| `acquire*` / `release*` | Hand out an item from a pool, or a fresh one when the pool is empty, and give it back: `Pool.Acquire` / `Pool.Release`. |

- A file name is the type or concept it holds, in lowercase words run together: `pricetable.go`, not `price_table.go`. An underscore only separates a suffix Go or the project gives meaning: `_test`, `_gen`, or a build tag such as `_windows` or `_debug`.
- A bitmask type is named `<Name>Flags`, and each of its bits `<Name>Flag<Bit>`: `PermissionFlags` holds `PermissionFlagRead`, `NotifyFlags` holds `NotifyFlagEmail`. A type whose values are exclusive is an enum and keeps a plain name: `SortOrder`, `SortOrderAscending`.

## Code

- Private methods go at the end of their file, after the exported ones.
- A helper used once or twice for a line or two is inlined at its call sites.
- Code lives in the file of the type it belongs to, even when only one feature uses it.
- In a file of several types, each type's methods follow its declaration, its exported methods before its private ones. A method lives in its own type's file, even when it serves a feature built around another type: `Store.applyDiscount` is in `store.go`, not `discount.go`, and the mailer's retry step `Mailer.scheduleRetry` is in `mailer.go`, not `retry.go`.
- One assignment per line: `s.reader.server = s` then `s.writer.server = s`, not `s.reader.server, s.writer.server = s, s`. The results of one call stay together: `job, ok := queue.Pop()`.
- When the toolchain is Go 1.27 or later, a generic operation on a type is a method with its own type parameters, not a package function taking the value as an argument: a service lookup is `registry.Lookup[Printer]()`, not `services.Lookup[Printer](registry)`. Check the module's Go version before shaping an API around a limitation of older Go.

## Tests

- A test of concurrent behaviour runs under `go test -race`, and is checked by breaking the code it guards and seeing it fail.

## Potential rules

- [1] Constants that describe one encoding are declared together in one `const` block, beside the type they encode, even when another file is where one of them gets used: `compressedBit` sits with `lengthBitCount`, `lengthMask` and `versionMask` in `header.go`, not alone in `reader.go`.
- [1] A type's constructor follows its declaration, before the type's methods, even a private constructor: `newBucket` comes right after the `bucket` struct, as `NewLimiter` follows `Limiter`, not at the end of `bucket.go` with the private methods.
- [1] A failure that does not depend on the call's arguments returns its sentinel error as it is, not wrapped with the operation and the name: a send with no mailer configured returns `mail.ErrNoMailer`, not `&mail.SendError{Op: "send", To: address, Err: ErrNoMailer}` through a helper.
- [1] A second type that grows its own set of methods in a file moves to a file of its own, named after it: `ReportView`, with its nine methods, is in `reportview.go`, beside `Report` in `report.go`.
- [1] A second type with functions of its own moves to a file named after it, as one with methods does, even when it has no methods: `settings`, with `loadSettings`, is in `settings.go`, not in `server.go` beside `Server`.
- [1] Each type is declared on its own, with its own comment, never grouped in a `type ( ... )` block.
- [1] A file holding a feature's types and functions together is named for the type it is built around, not for the feature: the connection pool, `Dial`'s handler and the idle sweep are in `connpool.go`, not `connreuse.go`.
- [1] Types that share the same fields and methods embed one private type holding them, instead of each repeating them: `Invoice`, `Receipt` and the other documents are `struct{ document }`, with `Number()` declared once on `document`.
- [1] A part that stands on its own, working on plain values and needing nothing else of its package, is a subpackage: the site's Markdown renderer works on text over an `fs.FS`, so it is `site/markdown`, `markdown.Render`, which a tool with no server imports, not a function beside the server.
- [1] A type's name says what it is on its own, not leaning on its package's name to say it: a ring queue is `RingQueue`, not `Queue` in a package `ring`.
- [1] A part implemented in a single file gets no package of its own, even when it stands on its own as the subpackage rule above describes: it goes beside parts of the same kind, in a package with a meaningful name, of which `common` is one choice, not the rule. The ring queue is `common.RingQueue`, not a package `ringqueue`.
- [1] When parts of one kind need nothing of their package but a base type, that type moves down to a shared package so the parts can have a package of their own, instead of the import cycle keeping them where they are: the money types `Currency` and `TaxRate` needed only `Precision` of `billing`, so `Precision` went to `common` and the money types to `billing/money`.
