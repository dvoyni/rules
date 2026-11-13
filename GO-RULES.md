# Go rules

## Naming

Lifecycle verbs come in fixed pairs. Pick the pair by what the code does, and keep each verb to its one meaning:

| Pair | Meaning |
|---|---|
| `new*` / `make*` | Constructors. `new*` returns a pointer, `make*` returns a value, as Go's `new` and `make` do. |
| `init` / `destroy` | Bring up and tear down something that already exists. A failed `init` destroys what it already initialized, in reverse order. `Start` / `Stop` run between them. |
| `acquire*` / `release*` | Hand out an item from a pool, or a fresh one when the pool is empty, and give it back: `Pool.Acquire` / `Pool.Release`. |

- A file name is the type or concept it holds, in lowercase words run together: `indextable.go`, not `index_table.go`. An underscore only separates a suffix Go or the project gives meaning: `_test`, `_gen`, or a build tag such as `_windows` or `_debug`.
- A bitmask type is named `<Name>Flags`, and each of its bits `<Name>Flag<Bit>`: `FeatureFlags` holds `FeatureFlagDepthClipControl`, `BufferUsageFlags` holds `BufferUsageFlagCopyDst`. A type whose values are exclusive is an enum and keeps a plain name: `CullMode`, `CullModeBack`.

## Code

- Private methods go at the end of their file, after the exported ones.
- A helper used once or twice for a line or two is inlined at its call sites.
- Code lives in the file of the type it belongs to, even when only one feature uses it.
- In a file of several types, each type's methods follow its declaration, its exported methods before its private ones. A method lives in its own type's file, even when it serves a feature built around another type: `Kernel.resolveSockets` is in `kernel.go`, not `socket.go`, and the World's snapshot step `World.takeSnapshot` is in `world.go`, not `snapshot.go`.
- One assignment per line: `k.graph.kernel = k` then `k.combiner.kernel = k`, not `k.graph.kernel, k.combiner.kernel = k, k`. The results of one call stay together: `slot, ok := q.Pop()`.
- When the toolchain is Go 1.27 or later, a generic operation on a type is a method with its own type parameters, not a package function taking the value as an argument: World access is `access.ReadCollection[Body]()`, not `world.Read[Body](access)`. Check the module's Go version before shaping an API around a limitation of older Go.

## Tests

- A test of concurrent behaviour runs under `go test -race`, and is checked by breaking the code it guards and seeing it fail.

## Potential rules

- Constants that describe one encoding are declared together in one `const` block, beside the type they encode, even when another file is where one of them gets used: gtb's `freeSlotGenerationBit` sits with `slotBitCount`, `slotMask` and `generationMask` in `id.go`, not alone in `entityset.go`. (Taught when `freeSlotGenerationBit` was declared on its own in `entityset.go`.)
- A type's constructor follows its declaration, before the type's methods, even a private constructor: gtb's `newCollection` comes right after the `Collection` struct, as `NewWorld` follows `World`, not at the end of `collection.go` with the private methods. (Taught when `newCollection` sat at the end of `collection.go`.)
- A failure that does not depend on the call's arguments returns its sentinel error as it is, not wrapped with the operation and the name: a write with no writable filesystem bound returns `platform.ErrNoWriteFS`, not `&fs.PathError{Op: "write", Path: name, Err: ErrNoWriteFS}` through a helper. (Taught when platform's writes wrapped `ErrNoWriteFS` through a `layer(operation, name)` helper.)
- A second type that grows its own set of methods in a file moves to a file of its own, named after it: gtb's `CollectionView`, with its nine methods and the `AnyCollectionView` every one of them is, is in `collectionview.go`, beside `Collection` in `collection.go`. (Taught when `CollectionView` grew the snapshot's read methods inside `collection.go`.)
- A second type with functions of its own moves to a file named after it, as one with methods does, even when it has no methods: mux's `registry`, with `findRegistry`, is in `registry.go`, not in `plugin.go` beside `Plugin`. (Taught when mux's registry was first declared in `plugin.go`, as ecs's is.)
- Each type is declared on its own, with its own comment, never grouped in a `type ( ... )` block. (Taught when `exclusiveBinding` and `bindings` were declared in one block in kernel's `socket.go`.)
- A file holding a feature's types and functions together is named for the type it is built around, not for the feature: gfx's frame pool, `Draw`'s handler and the acquire and present tasks are in `framepool.go`, not `framechain.go`. (Taught when gfx's frame chain was first put in `framechain.go`.)
- Types that share the same fields and methods embed one private type holding them, instead of each repeating them: gfx's `Buffer`, `Texture` and the other handles are `struct{ handle }`, with `Index()` declared once on `handle`. (Taught when gfx's eleven handles each declared `struct{ index, generation uint32 }` and their own `Index()`.)
- A part that stands on its own, working on plain values and needing nothing else of its package, is a subpackage: the gogpu plugin's WGSL preprocessor works on text over an `fs.FS`, so it is `gogpu/shader`, `shader.Preprocess`, which a tool with no GPU imports, not a function beside the plugin. (Taught when the preprocessor was first ported into package gfx.)
- A type's name says what it is on its own, not leaning on its package's name to say it: gtb's triple buffer is `SwapBuffer`, not `Buffer` in a package `triplebuffer`. (Taught when gtb's triple buffer type was first named `Buffer`, to avoid repeating the package name.)
- A part implemented in a single file gets no package of its own, even when it stands on its own as the subpackage rule above describes: it goes beside parts of the same kind, in a package with a meaningful name, of which `common` is one choice, not the rule. gtb's triple buffer is `common.SwapBuffer`, not a package `triplebuffer`. (Taught when gtb's triple buffer was first built as its own package `triplebuffer`, then when this rule, first written in gtb's `server/CLAUDE.md`, sent every such part to `common`.)
- When parts of one kind need nothing of their package but a base type, that type moves down to a shared package so the parts can have a package of their own, instead of the import cycle keeping them where they are: gtb's World Resources `Clock` and `TickCounters` needed only `FieldFlags` of `world`, so `FieldFlags` went to `common` and the Resources to `world/resources`. (Taught when keeping them in `world` was recommended because of the import cycle through `FieldFlags`.)
