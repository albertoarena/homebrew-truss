# Homebrew tap for Truss

The Homebrew tap for [Truss](https://trussphp.com), a live database structure
viewer and schema doctor. Structure only, never data.

## Not ready yet

**There is no formula here yet.** This repository is reserved for the `truss`
command-line binary, which has not been released. `brew tap` will succeed and
`brew install` will not find anything, so there is nothing to try today.

When the binary ships, installing it will be:

```sh
brew tap albertoarena/truss
brew install albertoarena/truss/truss
```

**Two commands, because Homebrew does not tap a repository for you.** Asked to
install from a tap it has not been given, it stops and tells you to tap it
explicitly first.

**The install name is fully qualified on purpose**, and tapping alone is not
enough to shorten it: tapping a repository does not grant whole-tap trust, so
the bare name is recognised and then refused. If you would rather type the
short form, trust the tap once:

```sh
brew trust albertoarena/truss
brew install truss
```

**`brew install albertoarena/truss` is not a shorter spelling of any of this,
and it is worth knowing why.** A name with one slash is not read as a tap.
Homebrew discards the owner and looks in its own core repository for a formula
called `truss`, which is not this one. The name belongs to whoever packages the
FreeBSD and Solaris syscall tracer of the same name, should anybody ever do so.

## Truss today

Truss is currently a Laravel package, installed with Composer:

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

Conventions for this repository, including how the formula is kept current and
what may be changed by hand, are in [AGENTS.md](AGENTS.md). A fuller
contributing guide follows once there is a formula to contribute to.

## Licence

MIT. See [LICENSE](LICENSE).
