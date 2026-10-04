# Contributing to FOCES Flag

Thank you for considering a contribution. This project is in its early setup
stage, so its codebase, development commands, and conventions may grow over
time. Please check the README and the current GitHub issue templates before
starting work.

## Ways to contribute

- Report a reproducible bug with the bug-report issue template.
- Suggest a feature or improvement with the feature-request issue template.
- Improve documentation, automation, or source code in a focused pull request.
- Help review open issues and pull requests.

For a new or substantial change, open an issue first so maintainers can discuss
the scope and direction before implementation.

## Development setup

At present, this repository does not contain application source code, a
`package.json`, or project-specific local test commands. Do not assume that a
build or test command exists. When application code and setup instructions are
added, this section should be updated to describe the supported tools and exact
commands.

The current CI workflow is prepared for a Node.js project: if a `package.json`
is added, it sets up Node.js 22 and runs `npm ci`. It then runs `lint`,
`typecheck`, and `build` only when those scripts are defined, followed by
`npm audit --audit-level=critical`. Run the applicable checks locally before
opening a pull request. The repository currently has no test script configured
in CI.

## Making a change

1. Check existing issues and pull requests; for larger changes, discuss the
   proposal in an issue first.
2. Create a focused branch from `main`. Suggested prefixes include `feat/`,
   `fix/`, `docs/`, and `chore/` (for example, `docs/security-guidance`).
3. Keep changes small and explain their purpose. Follow the existing style in
   the files you touch; avoid introducing dependencies or tools without a
   clear need.
4. Run the checks relevant to your change and describe what you ran. If a check
   cannot be run, say so rather than implying it passed.
5. Open a pull request against `main`. Complete the repository pull request
   template, link related issues, and include screenshots for user-facing
   changes when applicable.

Use a clear commit message. Conventional Commit-style messages are encouraged
(for example, `docs(readme): explain project status`); the current repository
does not enforce a commit-message format.

## Pull request checklist

- [ ] The change has a clear purpose and is limited to its stated scope.
- [ ] Related documentation has been updated.
- [ ] Relevant checks have been run, and their results are described in the PR.
- [ ] No secrets, credentials, private keys, or personal data have been added.
- [ ] The PR template is complete; UI changes include before/after screenshots
      when applicable.
- [ ] I have read and will follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Review and merging

Pull requests are reviewed by the repository's listed code owners. Please
respond constructively to review feedback. A change may be revised or declined
if it is out of scope, unsafe, unsupported, or inconsistent with the project's
direction. Do not merge your own change unless the maintainers have explicitly
authorized it.

## Security-sensitive changes

Do not disclose vulnerabilities in a public issue or pull request. Follow the
private reporting instructions in [SECURITY.md](SECURITY.md), and avoid
including exploit details in a public discussion until maintainers have
coordinated disclosure.