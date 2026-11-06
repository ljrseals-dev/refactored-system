# Grok Workspace Import — AI Agent Execution Plan

## Purpose

This pull request documents the proposed, reviewable workflow for importing the contents of `grok-workspace(1).zip` into this repository. The intended result is that an AI coding agent extracts the archive, preserves its relative directory structure, commits the extracted workspace on an import branch, and proposes that branch for integration into `main`.

**Important status:** this PR contains the import plan only. It does not claim that the ZIP has been transferred, extracted, or committed. The archive-content import must be performed by an agent or runner that can read the archive bytes and upload binary Git objects. Do not mark the import complete until the verification checklist below passes.

## Proposed agent workflow

1. **Inspect the source archive**
   - Locate `grok-workspace(1).zip` supplied by the project owner.
   - Record its SHA-256, byte size, and ZIP entry count before extraction.
   - List entries and reject absolute paths, parent-directory traversal (`..`), unsafe symlinks, and other paths that escape the temporary extraction directory.
   - Extract into a clean temporary workspace while preserving relative paths.
   - Do not recursively expand nested archives unless explicitly approved.

2. **Build an import manifest**
   - Record every extracted file's repository-relative path, byte size, SHA-256, and Git mode.
   - Preserve executable bits where present (`100755`); use `100644` for ordinary files.
   - Treat images, fonts, audio, archives, and other binary assets as bytes, not UTF-8 text.
   - Inventory screenshots and retain their original paths.

3. **Protect the existing repository**
   - Fetch the current `main` head and record its commit and tree SHAs.
   - Compare every incoming path against the existing tree.
   - Never silently overwrite a path already tracked by the repository. Rename colliding incoming documentation (for example, use `README.grok.md`) or stop for owner approval when a collision changes project behavior.
   - Keep all unrelated files and the existing commit history intact.

4. **Create the import branch and commit**
   - Create `import/grok-workspace` from the observed `main` head.
   - Upload each extracted file as a Git blob using base64 for binary-safe transfer.
   - Construct a Git tree on top of the recorded base tree, adding only the manifest paths that passed collision review.
   - Create the import commit with the exact message `Import Grok workspace`, with the original `main` commit as its parent.
   - Do not update `main` directly and do not force-push.

5. **Author the technical README**
   - Add `README.grok.md` at the repository root unless the owner approves a different non-conflicting path.
   - Explain the workspace mission, directory and subsystem inventory, prerequisites, launch sequence, and operational limitations.
   - Embed every verified screenshot from the archive using repository-relative Markdown paths and add a concise caption for each.
   - Record the import commit SHA and the archive checksum in the mission log. Do not invent descriptions for files whose purpose has not been established.

6. **Open a pull request into `main`**
   - Push the completed import branch and open a PR from `import/grok-workspace` to `main`.
   - Describe the extraction method, collision decisions, file counts, screenshot inventory, archive checksum, and verification results.
   - Prefer a merge commit so the import commit remains visible in history.
   - If `main` advances after the import branch is created, update the branch through a normal merge/rebase and repeat verification. Never force-push.

## Expected repository layout

The agent should preserve the archive's actual paths rather than inventing a new hierarchy. The known archive inventory includes functional areas such as `.grok/`, `src/`, `scripts/`, `public/`, `server/`, `screenshots/`, and `attachments/`; these names are an inventory guide, not a substitute for inspecting the ZIP manifest.

## Verification gate before merge

- [ ] Source archive checksum, byte size, and entry count recorded.
- [ ] Extraction completed without path traversal or unsafe links.
- [ ] Manifest accounts for every extracted file.
- [ ] Binary assets were uploaded without text conversion.
- [ ] Existing `main` files were preserved; collisions were explicitly handled.
- [ ] Import commit message is exactly `Import Grok workspace`.
- [ ] Import commit has the pre-import `main` commit as an ancestor.
- [ ] README exists and every embedded screenshot path resolves.
- [ ] Git tree and file inventory match the intended manifest.
- [ ] PR targets `main`; no force-push was used.

## Acceptance criteria

Merge only after the PR diff contains the extracted workspace contents and the README, and the verification gate is complete. Merging this planning-only PR by itself does **not** import the ZIP; it only records the agreed execution procedure.