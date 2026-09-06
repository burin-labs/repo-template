# repo-template

Bootstrap scaffolding for a new Burin Labs repository.

Every repository in this organization is covered by the organization ruleset
on its default branch: signed commits, linear history, squash or rebase
merges, and a passing status check named `CI status`. A repository whose
default branch does not exist yet cannot satisfy that check, so a brand-new
repository has to arrive with its first commit already in place.

Generating from this template does that. The generated repository starts
with a default branch, a workflow that reports `CI status`, and the shared
Dependabot and pull request conventions. Every later change arrives by pull
request, under the ruleset, with nothing bypassed.

"Create a new repository" in the organization `.github` repository has the
exact commands.

## What the template provides

- `.github/workflows/ci.yml`: Markdown and workflow lint, plus the
  `CI status` job the ruleset requires.
- `.github/dependabot.yml`: weekly grouped GitHub Actions updates with a
  seven-day cooldown.
- `.github/pull_request_template.md`: the pull request description contract.
- `.gitignore`: common editor and operating system noise.

## First pull request

A generated repository builds and tests nothing yet, because the template
does not know what language you are about to write. Open one pull request
that adds the real work and the job that checks it:

1. Add a job to `.github/workflows/ci.yml` that builds and tests the code.
2. Add that job to the `needs` list of the `ci-status` job.
3. Add the matching Dependabot ecosystem to `.github/dependabot.yml`.
4. Replace this README.

For a Harn package the build job is one call to the organization's reusable
`harn-package.yml` workflow. Its inputs are documented in the `.github`
repository.

## Why `CI status` is a single job

The ruleset names one required check for every repository, so a repository
cannot weaken its own gate by renaming or dropping a job. The `ci-status`
job is an aggregator: it runs after everything else, always, and fails when
any job it waited on failed. Repositories add jobs to `needs`; they do not
change what the ruleset asks for.

#### A heading level that markdownlint rejects, to prove the gate fails
