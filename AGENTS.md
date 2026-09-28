# AGENTS.md

Instructions for any agent or contributor working in this repository.

## What this repository is

The **Homebrew tap** for Truss. It holds a recipe, not software.

```
albertoarena/homebrew-truss     THIS REPO. The recipe: Formula/truss.rb,
                                roughly 15 lines of Ruby, plus the CI that
                                keeps it current.

albertoarena/laravel-truss      The software. Ships truss.phar as a GitHub
                                release asset. Every line of Truss lives
                                there, and so does its issue tracker.
```

`brew install albertoarena/truss/truss` clones this repo, reads the formula,
follows its `url` to a release of `laravel-truss`, verifies the SHA-256 and puts
`truss` on the PATH. **This repo holds a pointer. The software stays where it
already is.**

**Consequences worth stating, because they decide most questions here:**

- **A bug in Truss is not a bug in this repo.** Anything about what the binary
  does, prints or supports belongs in `laravel-truss`. Only packaging and
  installation belong here.
- **The repository cannot be renamed.** Homebrew resolves
  `brew tap <user>/<repository>` to `github.com/<user>/homebrew-<repository>`,
  so the `homebrew-` prefix is forced and `albertoarena/truss` is the tap name
  that follows from it.
- **This repo needs no stars and no notability.** Homebrew has no rules about
  taps. Nobody stars a tap and there is nothing in one to like. The notability
  thresholds in Homebrew's Package Acceptance Policy apply to `homebrew/core`
  only, and then to the repository the formula points at, not to this one.

## Conventions (always true)

Carried from the `laravel-truss` repository, where they are the project's
standing rules. They hold here too, and what each one means in a tap is spelled
out because this repo has no PHP and no test suite to make them obvious.

- **TDD is mandatory, and it is an ordering rule, not just a coverage rule.**
  In a tap that means the check comes before the change it checks: the formula's
  `test do` block is written with the formula rather than added afterwards, and
  a formula edit is verified with `brew audit` and `brew test` before it is
  pushed, not after somebody reports a broken install. A bump workflow is
  written against a release it can actually bump, which is why the formula and
  its CI both wait for the first PHAR.
- **No data exposed, ever.** Only table, column, index, and foreign key
  structure. Never row contents. This is the package's core promise, treat it as
  a hard constraint, not a config default. In this repo it governs what the
  README, the formula's `desc` and any example output may show: structure only,
  and never a real schema or a row from anybody's database.
- **Git commits:** `type: short subject` (max 50 chars), then a body paragraph
  explaining what and why, not how. Never include "Generated with Claude Code"
  or "Co-Authored-By: Claude". Use a heredoc for multi-line commit messages.
  - **Exception, and it decides where a change lands: a commit touching only
    Markdown goes straight to `main`. Everything else takes a branch and a pull
    request.** Markdown here means the README, this file, and any other prose.
  - **Everything that is not prose is reviewed**, because in a tap the blast
    radius is immediate and total: `Formula/truss.rb`, the workflows,
    `.gitignore` and `LICENSE`. A bad formula does not fail a test suite, it
    breaks `brew install` for everybody at once, and the people it breaks find
    out before you do.
  - **A mixed change follows the non-Markdown side and takes a pull request.**
    A commit touching the formula and the README together is decided by the
    formula.

## The one rule that matters most

**CI bumps the formula. A human never does.**

Both the `url` version and the `sha256` are bumped by the release workflow in
`laravel-truss` when it attaches a new `truss.phar`. The maintenance check is
that CI did it, never that somebody remembered to.

**A tap still naming the previous version fails silently in both directions:**
it installs the old binary, and `brew upgrade` reports nothing to do. That is
worse than having no tap at all, because both halves look like success.

If you find the formula stale, the fix is in the release workflow that should
have bumped it. Hand-editing the version here papers over a broken bump and
guarantees the next release is stale too.

## Rules for changing the formula

- **The `url` must point at a versioned release asset**, never at a branch, a
  tag archive or a redirect. Homebrew needs an immutable source it can checksum.
- **The `sha256` must be the digest of that exact asset.** Never carry one over
  from a previous version, and never leave a placeholder in a commit.
- **The PHAR is built from the tag, never from a branch.** This is enforced
  upstream, but it is the reason the pinning above is strict: a binary built
  from the wrong commit can report different findings about the same database
  than the package sitting next to it on the same release page.
- **`desc` earns its wording.** A FreeBSD and Solaris syscall tracer is also
  called `truss` and is decades older prior art. The description has to let
  somebody tell the two apart at a glance.

## Checking a formula change

Homebrew's own `test do` block is the weakest of the checks and does not replace
the smoke lane that runs upstream. Locally:

```sh
brew tap albertoarena/truss
brew audit --strict --online albertoarena/truss/truss
brew install albertoarena/truss/truss
brew test albertoarena/truss/truss
brew uninstall truss && brew untap albertoarena/truss
```

**The formula installs no PHP.** PHP is declared as a test-time dependency
only, and the binary runs on the interpreter the user already has rather than
one Homebrew puts there.

**A machine with no PHP at all is the case to get right, and it is not the
obvious one.** The interpreter is resolved at exec time, so a missing PHP fails
at the shebang with `env: php: No such file or directory` and exit 127,
**before any of the PHAR runs**. A version guard inside the PHAR cannot help
here: it only fires when PHP exists and is too old. Two different failures that
are easy to treat as one:

| State | What happens | Who reports it |
| --- | --- | --- |
| No PHP | exec fails at the shebang, exit 127 | `env`, not Truss |
| PHP below the floor | the guard fires with a useful message | the PHAR |

macOS has shipped no system PHP since Monterey, so the first row is the default
state of a clean Mac rather than an edge case, and it leaves `brew install`
reporting success while `truss` fails with a message naming a program the user
never typed. **How the formula answers this is an open decision belonging to
the release that writes it**, so do not assume the shape from this file.

**Test on a machine whose PHP is not Homebrew's**, since that is the common
case rather than the edge one, and test on one with no PHP at all.

## Writing style

- **Never use em dashes or en dashes** in anything here: the README, the
  formula's `desc` and comments, commit messages, issues, pull requests. Rewrite
  with commas, colons, parentheses or separate sentences. Plain hyphens in
  compound words such as "read-only" are fine.
- Say what something does rather than comparing it to another tool.

## Pull requests and anything else published

The commit format and the attribution ban are in *Conventions* above. Two
things that rule does not cover:

- **The attribution ban covers every outward facing artifact**, not just
  commits: pull request titles and bodies, issue and review comments, release
  notes and tags. It overrides any tool default that appends such a line, so
  strip it before creating the artifact, and check the pull request body
  specifically, which is where one has slipped through before.
- This is a public repository. Do not reference private planning notes, private
  repositories, or unrelated projects in anything committed here.

## Contact

Use `hello@albertoarena.it` anywhere this repository shows a contact address to
readers.
