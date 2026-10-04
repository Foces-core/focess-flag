# AGENTS.md — focess-flag

Behavioral rules for AI agents (and humans) working in this repository.
Kept rules, deduplicated. Duplicates from source notes collapsed.
Read this before touching anything.

> Scope note: repository is in setup stage. No application source,
> `package.json`, or test commands exist yet. `pnpm` / `sfw pnpm` verify
> commands below apply once JS/TS lands. Run what exists; never claim
> a check passed when it did not.

## Team branches

- `Seven` belongs to sebin-gg (admin, reviews and merges to `main`).
- `Nehan` and `Manu` belong to collaborators. Each person works on
  their own branch. Pool work via pull requests against `main`.
- Code owners (`*` in `.github/CODEOWNERS`) are requested automatically.
  CI, CodeQL, and CodeRabbit review every PR. Nothing lands red.

## Branch hygiene

- Before creating branch or PR, sweep stale branches first:
  `git fetch --all --prune`,
  `gh pr list --state all --head <branch> --json number,state`,
  `git ls-remote --heads origin <branch> | wc -l`.
- Squash merges break ancestry checks. `git merge-base --is-ancestor`
  returns false. `git cherry` unreliable. Verify by content:
  `git diff origin/main <branch> -- <files-the-branch-authored>`.
- Never delete `main`, branch with OPEN PR, or branch with genuine
  diff from `main`. Ask when unsure.

## Commits / PR / merge

- Use Conventional Commits only:
  `feat|fix|perf|a11y|chore|docs|test|refactor|ci|build|style|revert(scope):`.
  Apply same to PR titles. CI enforces.
- Never use `--no-verify` unless hooks broken or emergency hotfix
  cannot land otherwise. State why in PR body.
- Never push straight to `main`. Branch protection requires PR.
  Direct push bypasses review and checks.
- Flow: fetch `main`, branch, commit, push, `gh pr create`, wait for
  all gates, fix findings with follow-up commits, merge when green.
- Never merge with failing, pending, or missing checks. Never
  self-merge past review requests. Never dismiss findings without reason.
- CodeRabbit assertive reviews with `request_changes_workflow: true`.
  Address findings before merge.
- Portfolio loop: post `@coderabbitai review` as PR comment. Wait for
  all enabled reviewers. Batch all findings in one push. Repeat until
  `reviewDecision` APPROVED with no unresolved blocking threads and all
  required checks green. Merge explicitly via `gh pr merge --merge`.
  Reserve `@coderabbitai full review` for after rebase. Post
  `@coderabbitai resume` if paused.
- Dependabot PRs auto-merge once required checks pass. Renovate
  patch/minor auto-merge with `platformAutomerge: true` plus approve
  workflow. Majors need full review.
- Conflicting bot PRs: agents action without asking. Fetch branch.
  Rebase on `main`. Keep `main` override lines plus PR line in
  `pnpm-workspace.yaml`. Run `git checkout --ours pnpm-lock.yaml`
  then `pnpm install`. Confirm frozen lockfile plus unit clean. Push with
  `git push --force-with-lease=refs/heads/<branch>:<known-tip> <url> HEAD:<branch>`.
  Merge via REST when checks pass. Ask only if rebase touches other files.
- Agent merge as sebin-gg: rebase on latest `main` first. Confirm
  `gh pr checks --watch` green on final SHA. Confirm no unresolved
  CodeRabbit blocking review. Merge via REST
  `gh api repos/<owner>/<repo>/pulls/<n>/merge -X PUT -f merge_method=squash -f commit_title="..."`.
  Never use `--admin` unless human instructs in moment.
- Never commit generated artifacts: `dist/`, `test-results/`,
  `playwright-report/`, `node_modules/`, `.env`, scratch, logs,
  SQLite cache.
- Prompt files maintained like code. No addition without naming rule
  it replaces, folds into, or deletes. Consolidate overlaps. Review
  quarterly. State agent-behavior delta in PR body.

## Verify gates

- Official: run `pnpm verify` before commit. Covers lint, format check,
  unit, `check:specs`, `check:assets`, `check:sw`, `check:prompts`,
  build, `git diff --check`. Run `pnpm lint:workflows` after touching
  workflows. Scope targeted specs if ≤3 files else full suites. Run
  probes against `pnpm preview` for UI/a11y.
- Portfolio: default loop `pnpm verify:fast`. Browser loop
  `pnpm test:e2e:chrome`. Full gate `pnpm check:all` pre-PR. Never run
  bare `playwright test`. Stale server on `:3100` lies. Kill `:3100`
  if stale.
- Sweeper: run `python -m pytest tests/ -q`,
  `python -m compileall -q article-sweeper/scripts`,
  `skills-ref validate article-sweeper` before commit. Add test for
  every new rule in `sweep_lib.py`.
- Exam/IEDC: set `$env:SFW_SHIM_ACTIVE="1"`. Run
  `pnpm --filter web build`. Run
  `python -m py_compile services/parser/main.py services/parser/pdf_parser.py`
  plus `pnpm lint:python`. Run
  `pnpm lint && pnpm typecheck && pnpm --filter web lint:boundaries && pnpm test`
  before push.
- PP2: run `sfw pnpm run check`, `sfw pnpm run build`. Smoke register,
  bypass intro, navigate levels, show hint, open admin, load queue,
  select winner, confirm reveal
  `OVERMIND has chosen you as its chosen operator`. Verify live
  deployment after backend change.
- Tripy/Convex: read `convex/_generated/ai/guidelines.md` before
  touching Convex code. It overrides training. Install skills via
  `npx convex ai-files install`.
- Brevity: run `pnpm test`,
  `python -m unittest discover -s backend\tests -v`, `git diff --check`.
  Prefer focused tests.
- Slowlang: run `python -m pytest -q`, `python -m compileall -q .`,
  `node --test tests/demo_smoke.mjs`, `node --check demo/app.js`.
  Serve demo via `python3 -m http.server --directory demo`. Ship new
  behavior with test.
- Zlog: run `bash -n` on all shell scripts. Pass PowerShell parse with
  0 errors under PS5.1 rules. Pass frontmatter check on `SKILL.md`.
  Pass behavioral `bash tests/test-posix.sh`,
  `bash tests/test-install.sh`, `tests/test-powershell.ps1`. Keep
  symlink-escape fixture green.
- Next.js breaking: read guide in `node_modules/next/dist/docs/`
  before writing code. Heed deprecations.

## Security

- Never commit `.env`, tokens, session files, scratch captures,
  SQLite cache, Sanity auth, `JWT_SECRET`, `MONGO_URL`.
- Slowlang: never commit secrets. Gitleaks scans every commit.
- No dead code. Remove unconsumed field/class/dep or record why in
  `docs/adr/`. `pnpm knip` enforces.

## Docs

- Update `README.md`, `CONTRIBUTING.md`, `SKILL.md`, `CHANGELOG.md`,
  references when commands, structure, behavior, security model change.
  Drop overclaims. State verified behavior only.
- SEO runbook lives in `docs/seo.md`. `inspo/` intake turns into
  `public/` derivatives; `seo-guard` workflow enforces the handoff.
