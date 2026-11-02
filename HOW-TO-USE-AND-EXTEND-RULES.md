# Rules: how to use and extend them

This folder holds rules shared between projects:

- `GENERAL-RULES.md`: rules for any project, in any language.
- `<LANGUAGE>-RULES.md` (`GO-RULES.md`, ...): rules for any project in that language.

A project's own rules live in that project's `CLAUDE.md`, beside the code they govern. Where a project rule and a shared rule disagree, the project rule wins.

## Using the rules

Follow every rule in every rules file the project loads. A **potential rule** is a lesson not yet confirmed: apply it, and say so to the user in your reply, naming the potential rule, so they can confirm it or drop it.

## Extending the rules

A user correcting how you worked is a **lesson**. Record every one:

1. **First correction:** add it as a potential rule, one line under `## Potential rules` in the file it belongs to, with the correction that taught it.
2. **Same correction again:** promote it. Move it into the file's rules and out of `## Potential rules`.
3. **Confirmed or dropped:** when the user confirms a potential rule you applied, promote it; when they drop it, delete it.

A user stating a rule outright ("always", "from now on", "never") makes it a rule at once.

Place each rule by how far it reaches:

- only this project → the project's `CLAUDE.md`
- every project in one language → `<LANGUAGE>-RULES.md`, created when the first such rule arrives
- every project → `GENERAL-RULES.md`

Write a rule as the behaviour to follow, with a short example from the code when one makes it sharper. A change to a shared file is committed in this folder's own repository.

## Adding the rules to a project

Add this repository as a git submodule named `rules` at the project root, and import the files from the root `CLAUDE.md`:

```
@rules/HOW-TO-USE-AND-EXTEND-RULES.md
@rules/GENERAL-RULES.md
@rules/GO-RULES.md
```

Import each language file the project uses, and only those.
