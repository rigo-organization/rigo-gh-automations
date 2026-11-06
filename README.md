# RIGO GitHub Automations

Reusable GitHub Actions workflows for RIGO projects.

## Available workflows

- `issue-code.yml`: prefixes issue titles with `[PREFIX-NUMBER]`.
- `pr-validation.yml`: validates PR title, branch name and closing issue reference.
- `release.yml`: publishes a GitHub Release with auto-generated notes for an existing pushed tag.

## Setup

1. Create the `rigo-gh-automations` repository in your GitHub organization, preferably public for simple cross-repository reuse.
2. Push these files to its default branch.
3. Create a release/tag `v1` in this automation repository (e.g. `git tag v1 && git push origin v1`).
4. Copy the caller files under `examples/despachao-backend/.github/workflows/` into the matching paths of your application repository.
5. Replace `YOUR_ORG` with the real GitHub organization or owner.
6. Merge the caller files into the default branch of the application repository.
7. Ensure Actions are enabled and repository Actions settings permit reusable workflows from this owner. The caller's `GITHUB_TOKEN` needs `issues: write` for issue updates and `contents: write` for releases. Repository/organization workflow token policies may also limit permissions.

## Examples

- Issue `#12` in despachao-backend becomes `[DPC-BE-12] ...`.
- Branch `feature/DPC-BE-12-add-customer`.
- PR title `[DPC-BE-12] Add customer registration`.
- PR body includes `Closes #12` (same repository).
- `git tag v1.0.0 && git push origin v1.0.0` creates a GitHub Release for that tag.

## Notes

Issue numbers are scoped to repositories; use different prefixes per repository to avoid collisions.

The release workflow creates a GitHub Release, **not** a Docker image or deployment. Add a separate deployment workflow when needed.

The PR validation checks naming and references only; it does **not** compile or test Quarkus or React code. Add build/test workflows separately.

These workflows are intended for trusted internal repositories. PR validation uses the read-only pull_request event and does not check out untrusted code.

Changing `@v1` requires updating or moving the central tag intentionally. For strict reproducibility pin a full commit SHA instead.
