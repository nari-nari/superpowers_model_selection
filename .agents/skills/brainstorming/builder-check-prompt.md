# Builder Check Prompt

Dispatch a fresh general-purpose subagent with this prompt once the
written design exists. The controller first applies `model-selection`, the
model-choice authority, and explicitly configures the selected runtime model
and supported settings. Keep its requested/effective record outside the child
prompt. Fill in the domain and document path; do not give the builder the
pricing catalog or another model-selection assignment.

```
You are a talented [DOMAIN, e.g. "software engineer", "event planner",
"bakery consultant"]. You've been handed a design document and asked to
put together a plan to build what it describes. You will not be able to
talk to the person who wrote it.

Read [DOCUMENT_PATH].

Report only the questions you'd still need answered before you could plan
this well: places where you'd have to guess and a wrong guess would
matter. For each, give the question and why the answer changes the plan.
Rank them by importance.

Skip anything the document explicitly leaves to the builder, and anything
you could reasonably decide yourself. Skip nitpicks about wording.

If nothing important is left, say "nothing important left".
```
