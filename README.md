# xtestlib

A source library for testing X-Lang package dependencies.

Package ID: `github.com/xespirit/xtestlib`. Version: `0.1.0`.
The public module is `xtestlib`, provided by `src/xtestlib.xl`.

## Check the library

With the sibling `xlang` and `xlc` executables installed and on `PATH`, run
from this directory:

```sh
xlang build
xlang test
```

`build` checks the library without requiring `main`. `test` runs the library's
embedded test. Generated output goes under `build/xlang/`; the project tool
also generates `XLang.lock`. Keep the existing `LICENSE` with the source.

## Use a local checkout

Add this table to a consuming project's `XLang.toml` when the project is next
to this checkout:

```toml
[dependencies.xtestlib]
id = "github.com/xespirit/xtestlib"
path = "../xtestlib"
```

## Use GitHub

Publish these files to `https://github.com/xespirit/xtestlib` and create the
Git tag `v0.1.0` at a revision whose manifest declares version `0.1.0`.
Then add this table to the consuming project's `XLang.toml`, instead of the
local dependency table:

```toml
[dependencies.xtestlib]
id = "github.com/xespirit/xtestlib"
git = "https://github.com/xespirit/xtestlib.git"
version = "^0.1.0"
```

Git fetching requires a libgit2-enabled build of `xlang`. The package ID must
match this library's manifest. The caret requirement accepts compatible
`0.1.x` releases; use `"0.1.0"` for an exact version.

Use the public function in the application's `src/main.xl`:

```xlang
use xtestlib

const {
    Left = 20
    Right = 22
    ExpectedSum = 42
}

fn main() {
    let actual = xtestlib.add(Left, Right)

    assert actual == ExpectedSum
}
```

From the consuming project, run:

```sh
xlang fetch
xlang run --locked
```

After fetching, `xlang run --locked --offline` reuses the cached source without
network access. Commit the application's generated `XLang.lock` when you want
other checkouts to use the same dependency revision.
