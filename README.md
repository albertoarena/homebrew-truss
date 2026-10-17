# Homebrew tap for Truss

The Homebrew tap for [Truss](https://trussphp.com): a live database structure
viewer that renders your schema as an ER diagram (ERD), and a schema doctor.
Structure only, never data.

## Install

```sh
brew install albertoarena/truss/truss
```

**One command is enough.** Given the fully qualified name, Homebrew taps this
repository itself the first time and installs the formula, so there is no
separate `brew tap` step.

**The install name is fully qualified on purpose**, and tapping alone is not
enough to shorten it: tapping a repository does not grant whole-tap trust, so
the bare name is recognised and then refused. If you would rather type the
short form, tap and trust it once:

```sh
brew tap albertoarena/truss
brew trust albertoarena/truss
brew install truss
```

**`brew install albertoarena/truss` is not a shorter spelling of any of this,
and it is worth knowing why.** A name with one slash is not read as a tap.
Homebrew discards the owner and looks in its own core repository for a formula
called `truss`, which is not this one. The name belongs to whoever packages the
FreeBSD and Solaris syscall tracer of the same name, should anybody ever do so.

## PHP

**Truss needs PHP 8.2 or newer, and this formula does not install it.** The
`truss` binary runs on the PHP you already have, whether that comes from
Homebrew, Herd or anywhere else on your PATH. macOS ships no PHP, so on a clean
Mac `truss` stops with:

```
Truss needs PHP. Run: brew install php
```

and exits 2. If the PHP it finds is older than 8.2, Truss says which version it
found and which one it needs.

## Truss as a Laravel package

Truss is also a Laravel package, installed with Composer:

```sh
composer require --dev albertoarena/laravel-truss
```

- Documentation: [trussphp.com](https://trussphp.com)
- Package: [albertoarena/laravel-truss](https://github.com/albertoarena/laravel-truss)

## What lives here, and what does not

This repository holds a recipe, not software:

| | |
| --- | --- |
| **This repository** | The formula. A pointer: a URL to a release asset and the SHA-256 that verifies it. |
| **[albertoarena/laravel-truss](https://github.com/albertoarena/laravel-truss)** | Truss itself, and the release the formula points at. |

`brew install albertoarena/truss/truss` reads the formula here, follows it to a
release of `laravel-truss`, checks the download against the pinned SHA-256 and
puts `truss` on the PATH.

**So please report issues where they belong.** Anything about what Truss does,
prints or supports goes to the
[package's issue tracker](https://github.com/albertoarena/laravel-truss/issues).
Open an issue here only for the tap itself: a formula that will not install, a
version that will not upgrade, or a checksum that does not match.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The full conventions for this
repository, including how the formula is kept current and what may be changed
by hand, are in [AGENTS.md](AGENTS.md).

## Licence

MIT. See [LICENSE](LICENSE).
