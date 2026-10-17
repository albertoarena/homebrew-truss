# Contributing

This repository is the Homebrew tap for Truss. It holds one formula,
`Formula/truss.rb`, which points at a `truss.phar` release asset of
[albertoarena/laravel-truss](https://github.com/albertoarena/laravel-truss) and
pins its SHA-256. Truss itself lives in that repository.

The full rules for this repository are in [AGENTS.md](AGENTS.md). This file is
the short version for somebody opening an issue or a pull request.

## Where to report what

- **What Truss does, prints or supports:** the
  [laravel-truss issue tracker](https://github.com/albertoarena/laravel-truss/issues).
- **Installing it with Homebrew:** here. A formula that will not install, a
  version that will not upgrade, a checksum that does not match.

Please include the output of `brew config` and `which -a php` with an install
problem. Never include rows from a database, or a schema that is not yours to
share: an issue about installing needs neither.

## The version is not edited by hand

**The release workflow in laravel-truss bumps the formula's `url` and `sha256`
when it publishes a new `truss.phar`.** A pull request that changes either one
by hand will be closed, even if the new values are correct. If the formula
names an old version, open an issue instead: the fix belongs in the release
workflow, or the next release goes stale too.

## Changing the formula

Every change except prose goes through a pull request, and CI runs
`brew audit --strict --online`, `brew install` and `brew test` on macOS and
Linux before it can merge. Run the same checks locally first:

```sh
brew tap albertoarena/truss
brew audit --strict --online albertoarena/truss/truss
brew install albertoarena/truss/truss
brew test albertoarena/truss/truss
brew uninstall truss && brew untap albertoarena/truss
```

To test your own copy of the formula rather than the published one, put it in a
scratch tap made with `brew tap-new`, and remove that tap afterwards.

**Test on a machine whose PHP is not Homebrew's, and on one with no PHP at
all.** The formula installs no PHP, so both are cases a real user is in, and
`brew test` on a CI runner covers neither.

## Commits

`type: short subject`, at most 50 characters, then a paragraph on what changed
and why. Plain hyphens are fine, but please no em or en dashes, in commits or
anywhere else in this repository.

## Contact

[hello@albertoarena.it](mailto:hello@albertoarena.it)
