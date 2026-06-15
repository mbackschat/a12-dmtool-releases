# Worked examples — the dmtool CLI, verb by verb

Six end-to-end walkthroughs (command + real captured output + narration) covering **every verb**. They
are executable docs — kept in sync with the live CLI by a CI gate in the source repo.

| Demo | Shows |
|---|---|
| [`cli-tour`](cli-tour.md) | discover + read — `manifest` · `operators` · `patterns` · `diagnostics` · `schema` · `model describe`/`validate` · `rule read`/`explain` |
| [`cli-edit-loop`](cli-edit-loop.md) | author/modify rules & computations, `batch`, the re-express loop |
| [`cli-structure-edit`](cli-structure-edit.md) | fields/groups/typedefs/includes/config + the safe-delete gate + the refactors |
| [`cli-apply`](cli-apply.md) | the atomic multi-op session — rollback + correctives + in-session refactors |
| [`cli-runtime`](cli-runtime.md) | `model eval` / `rule test` / `model compute` against document instances **(JVM only — not in the native binary)** |
| [`cli-version`](cli-version.md) | `--version`, `manifest.version`, the model-version compatibility policy |

> The commands call the bare `dmtool` (what the plugin/raw install puts on PATH). The **discovery** verbs
> (`manifest`, `operators`, `patterns`, `schema`, `--help`) run with no model; the **model-based** ones
> reference the project's own sample models, so read them to learn the shape of each call and its output.
