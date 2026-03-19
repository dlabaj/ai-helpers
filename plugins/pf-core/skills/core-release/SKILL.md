---
name: core-release
description: Guides through performing a PatternFly Core release for patternfly/patternfly. Use when the user wants to release PatternFly core, run a core release, publish @patternfly/patternfly, or follow the core release process.
---

# PatternFly Core Release

Guides the release process for [patternfly/patternfly](https://github.com/patternfly/patternfly) (HTML/CSS implementation). The canonical instructions are in the [Core Release document](https://docs.google.com/document/d/1jJ2RMltw8RKgLDzk3ptVjrQydtWYilplnvd1hrUyIZM/edit). **If the user has access, paste or summarize the doc content so steps below can be verified and completed accurately.**

## Prerequisites

- Node.js v20.18.3 or greater
- [Corepack](https://nodejs.org/api/corepack.html) enabled: `corepack enable`
- Clone of [patternfly/patternfly](https://github.com/patternfly/patternfly) with write access (or fork + upstream)
- GitHub and npm credentials configured

## Release Workflow

Copy this checklist and work through each step. Check off items as you complete them.

```
Core Release Progress:
- [ ] 1. Pre-release verification
- [ ] 2. Version bump / changelog
- [ ] 3. Build and test
- [ ] 4. Publish / semantic-release
- [ ] 5. Post-release verification
```

---

### Step 1: Pre-release verification

- Ensure you're on the correct branch (`main` for standard releases; `v5` for v5 prereleases per `release.config.js`)
- Pull latest: `git pull origin main` (or appropriate branch)
- Dependencies are clean: `yarn install`
- All tests pass locally

---

### Step 2: Version bump and changelog

- PatternFly Core uses [semantic-release](https://github.com/semantic-release/semantic-release) with Angular commit convention
- Version is determined from commits; ensure commits follow [Conventional Commits](https://www.conventionalcommits.org/):
  - `feat:` → minor bump
  - `fix:` → patch bump
  - `BREAKING CHANGE:` in footer → major bump
- If manual changelog updates are needed per your process, make them before the release step

---

### Step 3: Build and test

```bash
yarn build
yarn serve   # optional: verify locally at http://localhost:8001
yarn a11y    # accessibility audit (run in separate terminal after serve)
```

- Run `yarn a11y` and address any violations related to your changes
- For visual changes to full-page examples: `yarn screenshots` (after `yarn build && yarn serve`)

---

### Step 4: Publish (semantic-release)

The repo uses semantic-release with `release.config.js`:

- **main** → `prerelease` channel
- **v5** → `prerelease-v5` channel

Publish is typically CI-triggered (e.g., push to `main`). **Check the [Core Release document](https://docs.google.com/document/d/1jJ2RMltw8RKgLDzk3ptVjrQydtWYilplnvd1hrUyIZM/edit) for the exact steps** — the package.json does not include a `release` script; semantic-release may run in CI.

- Ensure `dryRun` is `false` in `release.config.js` for a real release (or use appropriate CI workflow)
- Semantic-release will: analyze commits, generate notes, create GitHub release, publish to npm from `dist/`

---

### Step 5: Post-release verification

- Confirm release on [GitHub Releases](https://github.com/patternfly/patternfly/releases)
- Confirm package on npm: `npm view @patternfly/patternfly version`
- If Core changes affect PF-React components, create follow-up issues in [patternfly/patternfly-react](https://github.com/patternfly/patternfly-react) per [Core contribution guidelines](https://pf-core-staging.patternfly.org/contribution)

---

## Reference

- **Pasted instructions**: If content from the Google Doc has been pasted into [reference.md](reference.md), follow those steps as the authoritative source.
- **Canonical instructions**: [Core Release document](https://docs.google.com/document/d/1jJ2RMltw8RKgLDzk3ptVjrQydtWYilplnvd1hrUyIZM/edit)
- **Repo**: https://github.com/patternfly/patternfly
- **Contribution guide**: https://pf-core-staging.patternfly.org/contribution
- **Release config**: `release.config.js` in repo root

When helping the user, ask them to paste or summarize the Google Doc steps if they differ from the above, so the workflow can be updated.
