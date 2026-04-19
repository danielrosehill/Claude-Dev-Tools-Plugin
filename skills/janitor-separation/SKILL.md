---
name: janitor-separation
description: Separate concerns in the repository — split tangled code/docs/config into properly scoped directories. Use when the user asks to separate layers or split mixed concerns.
---

Assess the current structure of the repository by scanning its contents.

The user's preference, you can assume, is that code and other elements should always be separated at a top-folder level. This means that if the app contains a folder for planning notes, reference material, docs, or another group of or another path that is not deployable code or code required for the operation of the application, then that should never be nested or at the same level as the code itself.

Refactor accordingly if required. But if you identify that a refactor for this purpose is necessary, then ensure that moving the base level path for the code would not interfere with deployment. If you're not sure about whether this might be the case, then always consult the user to obtain approval for any reformatting of what may be a deployable code base parent level.