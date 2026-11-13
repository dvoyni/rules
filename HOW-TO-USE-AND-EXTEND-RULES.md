# Rules: how to use and extend them

A project's own rules live in its `AGENTS.md` files, beside the code they govern. Where a project rule and a shared rule disagree, the project rule wins.

## Using the rules

Follow every rule in every rules file the project loads. A **potential rule** is a lesson not yet confirmed: apply it, and say so to the user in your reply, naming the potential rule, so they can confirm it or drop it.

## Extending the rules

A user correcting how you worked is a **lesson**. Record every one:

1. **First correction:** add it as a potential rule, one line under `## Potential rules` in the file it belongs to.
2. **Same correction again:** promote it. Move it into the file's rules and out of `## Potential rules`.
3. **Confirmed or dropped:** when the user confirms a potential rule you applied, promote it; when they drop it, delete it.

A user stating a rule outright ("always", "from now on", "never") makes it a rule at once.

Place each rule by how far it reaches:

- only this project → the project's `AGENTS.md` nearest the code it governs
- every project in one language → `<LANGUAGE>-RULES.md`, created when the first such rule arrives
- every project → `GENERAL-RULES.md`

Write a rule as the behaviour to follow, with a short example when one makes it sharper. In a project's own file, the example comes from the project's code, and a potential rule ends with the correction that taught it: `(Taught when ...)`.

A shared file is **synthetic**, so it can be shared under any NDA: every example, name and story in it, and in its repository's commit messages, is invented. Carry a lesson into it with an example made up for it, in a domain such as a bookshop or a mail server, and leave out the correction that taught it. A change to a shared file is committed in this folder's own repository.

## Adding the rules to a project

Add this repository as a git submodule, at `rules/` in the lines below, and point the project's root `AGENTS.md`, which every agent reads, at this folder's:

```
Before any work, read and follow rules/AGENTS.md.
```

An agent that reads only its own file gets a one-line pointer beside each `AGENTS.md`. For Claude Code that is a `CLAUDE.md` holding `@AGENTS.md`, which imports it. The root one also imports this folder's `CLAUDE.md`, which loads the shared rules without being asked for, and may import the file of each language the project is written in:

```
@AGENTS.md
@rules/CLAUDE.md
@rules/GO-RULES.md
```
