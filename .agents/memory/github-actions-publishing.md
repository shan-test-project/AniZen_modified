---
name: GitHub Actions publishing fallback
description: Durable workflow for publishing and triggering builds when Git HTTPS auth is unavailable.
---

When a GitHub token can authenticate to the REST API but Git HTTPS rejects it, use the Git Database API to create blobs, a tree, a commit, and update the target branch ref, then dispatch and monitor the workflow through REST.

**Why:** The repository token may have API permissions while Git transport still returns invalid credentials; repeatedly retrying Git push does not repair that mismatch.

**How to apply:** Keep the token out of output, use the repository's current branch head as the commit parent, and publish only the intended files in the new tree. For AniZen_modified, resolve the repository default branch through the API rather than assuming `main`.

For `actions/upload-artifact`, do not use shell syntax such as `${VERSION}` in the action's `path:` input; action inputs are not shell-expanded. Use a GitHub expression or a validated glob such as `dist/*.apk`.

**Why:** A successful APK build can still fail the workflow when the upload action receives a literal variable path and finds no files.

**How to apply:** Expand shell variables only inside `run:` steps, validate the copied artifact there, and pass a glob or `${{ env.VERSION }}` to upload-artifact.