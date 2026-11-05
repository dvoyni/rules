# Go rules

## Naming

Lifecycle verbs come in fixed pairs. Pick the pair by what the code does, and keep each verb to its one meaning:

| Pair | Meaning |
|---|---|
| `new*` / `make*` | Constructors. `new*` returns a pointer, `make*` returns a value, as Go's `new` and `make` do. |
| `init` / `destroy` | Bring up and tear down something that already exists. A failed `init` destroys what it already initialized, in reverse order. `Start` / `Stop` run between them. |
| `acquire*` / `release*` | Hand out an item from a pool, or a fresh one when the pool is empty, and give it back: `Pool.Acquire` / `Pool.Release`. |

- A file name is the type or concept it holds, in lowercase words run together: `indextable.go`, not `index_table.go`. An underscore only separates a suffix Go or the project gives meaning: `_test`, `_gen`, or a build tag such as `_windows` or `_debug`.

## Code

- Private methods go at the end of their file, after the exported ones.
- A helper used once or twice for a line or two is inlined at its call sites.
- Code lives in the file of the type it belongs to, even when only one feature uses it.
- One assignment per line: `k.graph.kernel = k` then `k.combiner.kernel = k`, not `k.graph.kernel, k.combiner.kernel = k, k`. The results of one call stay together: `slot, ok := q.Pop()`.

## Tests

- A test of concurrent behaviour runs under `go test -race`, and is checked by breaking the code it guards and seeing it fail.

## Potential rules

- In a file of several types, each type's methods follow its declaration, its exported methods before its private ones, and a method lives in its own type's file: `Kernel.resolveSockets` is in `kernel.go`, not `socket.go`. (Taught when `socket[T]`'s methods sat apart from it in kernel's `socket.go`, and `resolveSockets` was left there; it narrows "Private methods go at the end of their file".)
- Each type is declared on its own, with its own comment, never grouped in a `type ( ... )` block. (Taught when `exclusiveBinding` and `bindings` were declared in one block in kernel's `socket.go`.)
- A feature that spans several types lives in its own file, their methods for it included, instead of spread across their files: Save, Load and the Ref and keyed-map methods they use are in `serialization.go`. (Taught when serialization was first spread over `world.go`, `refs.go` and `entity.go`; it narrows "Code lives in the file of the type it belongs to".)
