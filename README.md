# Homebrew tap for Truss

The Homebrew tap for [Truss](https://trussphp.com), a live database structure
viewer and schema doctor. Structure only, never data.

## Not ready yet

**There is no formula here yet.** This repository is reserved for the `truss`
command-line binary, which has not been released. `brew tap` will succeed and
`brew install` will not find anything, so there is nothing to try today.

When the binary ships, installing it will be:

```sh
brew install albertoarena/truss/truss
```

## Truss today

Truss is currently a Laravel package, installed with Composer:

```sh
composer require --dev albertoarena/laravel-truss
```

- Documentation: [trussphp.com](https://trussphp.com)
- Package: [albertoarena/laravel-truss](https://github.com/albertoarena/laravel-truss)
