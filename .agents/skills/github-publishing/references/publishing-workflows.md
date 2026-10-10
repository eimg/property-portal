# Publishing workflows

## Independent projects

Publish each project from its own Git root: `api/`, `app/`, and `mobile/` if that repository is later added. Read that project's README and agent guidance first. Keep its existing commit history and branch. Do not create a shared package or combine project histories.

For a project without a GitHub remote, create the specifically chosen repository from that project directory with `gh repo create OWNER/NAME --source=. --remote=origin --push --private` or `--public`, according to the user's selected visibility. For an existing GitHub repository, inspect it with `gh repo view OWNER/NAME`; add or use the intended remote and push the current branch without force. If an `origin` already points elsewhere, preserve it and ask which destination/remote to use rather than replacing it. Do not treat an authentication failure, network failure, or ambiguous repository as permission to retry against another target.

## Coordination root and submodules

The root repository contains coordination documents (including `SPEC.md`, `AGENTS.md`, and `README.md`) and gitlinks to the independent child repositories. Publish children first so each submodule URL resolves to a real GitHub repository. Include a future mobile project only after it exists as its own Git repository.

When turning a root that already contains nonempty child checkouts into a superproject, `git submodule add` cannot safely be assumed to adopt those directories. Preserve each checkout and its ignored local files. Confirm each child is clean and pushed, then move the existing child directory to a clearly named sibling backup path, checking that the backup destination does not already exist. Keep that backup until the new submodule checkout is verified; do not delete or overwrite it as part of publishing.

Initialize or use the root Git repository separately. Review and commit only root-owned coordination files first, excluding child checkouts and local-only files. Create/push the selected root GitHub repository. Then add each child using its actual GitHub clone URL, for example:

```sh
git submodule add https://github.com/OWNER/API_REPO.git api
git submodule add https://github.com/OWNER/APP_REPO.git app
# Add mobile only when its independent repository exists.
git submodule add https://github.com/OWNER/MOBILE_REPO.git mobile
```

Use the URL format and owner that match the configured remotes. Verify `.gitmodules` records the intended paths and URLs, each submodule has the expected branch/commit, and `git submodule status` resolves all entries. Review that the root diff contains mode `160000` gitlinks for the projects, not copied source trees, credentials, databases, or build output. Commit `.gitmodules` and the gitlinks in the root and push the root branch. A fresh clone should be usable with `git clone --recurse-submodules` (or `git submodule update --init --recursive`).

If a move, checkout, remote, visibility, or destination is ambiguous, stop before the affected mutation and ask a focused question. Preserve backups and existing repositories until the root's submodule check succeeds.
