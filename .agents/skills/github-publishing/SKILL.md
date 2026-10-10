---
name: github-publishing
description: Publish this workspace's independent projects to GitHub and manage the coordination root as a superproject with those projects as submodules.
---

# GitHub Publishing

Use this skill when asked to create, connect, or publish GitHub repositories for this workspace. Treat `api/`, `app/`, and any future `mobile/` as independent projects with their own Git history, remotes, and GitHub repositories. The coordination root is a separate repository that tracks those projects as Git submodules; it does not absorb their source or history.

Read [references/publishing-workflows.md](references/publishing-workflows.md) for the child-project and root-superproject workflows.

Before changing GitHub or pushing:

- Inspect the target directory, its Git root, current branch, remotes, working tree, and ignored local files. Read its `README.md` and `AGENTS.md` when present.
- Run `gh auth status` and stop if the account is not authenticated. Do not log, print, or inspect token values. Tell the user to re-authenticate with `gh auth login -h github.com`.
- Resolve the GitHub owner, repository name, and visibility from existing remotes or explicit user choices. Ask only for missing choices that affect the destination or public exposure.
- Preserve existing history and remotes. Never force-push, rewrite history, overwrite an existing repository, or change visibility without explicit authorization.
- Do not publish uncommitted changes implicitly. If a target has changes, explain what is pending and ask whether to include them or leave the publication stopped.
- Check the exact files that would be committed for credentials, local databases, generated output, and other local-only data. Respect project `.gitignore` files; do not use a blanket `git add -A` without reviewing its effect.

When the user asks to publish, that authorizes the requested GitHub repository creation and push once the destination and visibility are clear. It does not authorize unrelated repository changes. After a successful push, report the repository URL, branch, and whether the coordination root contains the resulting submodule pointer.
