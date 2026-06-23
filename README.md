# Tandocs - Tandem developer docs

## How We Work

- At Tandem, you're responsible for every line of code you write. AI usage is expected, but be prepared to defend a variable name or a DB migration.
- Each project has a Project Lead (PL). If you don't know who this is, ask.
- Innovate! We're always open to new workflows and stuff. Everything is changing so quickly at the moment and we want to find cool ways to use AI while keeping control of the code.

## Contributing

### Pull Requests

- All Pull Requests must have at least 1 approval before merging.
- PR descriptions can use AI but keep them human-readable and relevant. AI has a tendency to list a load of things that can be inferred from looking at the code - no real person wants to read this. Focus on functionality and non-obvious changes.
- If your PR makes changes to the frontend, please include screenshots.
- Keep Claude out of the commit attributions.

### Reviews

- Make sure to request a review from your Project Lead and any other relevant parties on your PR.
- If changes are requested, make sure to re-request a review after making them.

### Documentation

- Each project has `ai-docs/` in the `.gitignore`. This is where you can put your own documentation to feed coding assistants without polluting the rest of the team's docs.
- Only include AI-generated documentation in the codebase if it contains something non-obvious from the code, such as a usage pattern that coding assistants may miss even if it’s documented in `CLAUDE.md`. Otherwise, prefer enforcing this through tooling such as `eslint` rules rather than generating verbose documentation that risks going stale quickly.

### Blocked?

- If blocked waiting for reviews on Branch X, feel free to create a new branch Y from Branch X. Use your best judgement here - if Branch X’s PR is really big, touches lots of files, and is likely to generate many comments or changes before merging, you’ll need to merge those changes through the working branch into Branch Y and handle the conflicts.
- When Branch X is merged into the working branch, you can then merge that branch into Branch Y, and your PR will not include all the changes from Branch X.

> [!IMPORTANT]
> If you push Branch Y and make a PR before Branch X gets merged, that PR will show the changes from both X and Y. To remedy this, rebase onto the working branch with the following command while in Branch Y:
>
> ```bash
> git rebase <workingbranch>
> ```
