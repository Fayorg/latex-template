## GitHub Build Version

The template can display a short build version from a GitHub commit hash:

```tex
\ProjectVersion
```

If no hash is configured, `\ProjectVersion` prints nothing. You can provide a
local fallback:

```tex
\ProjectVersion[local]
```

Set the hash manually with project metadata:

```tex
\ProjectSetup{github hash = abcdef1234567890}
```

The aliases `github_hash` and `githubhash` also work.

The rendered value uses the first seven characters, equivalent to:

```tex
Version: \texttt{abcdef1}\\
```

For GitHub Actions, generate a `build-version.tex` file before compiling:

```sh
printf '\\ProjectSetup{github hash = %s}\\n' "$GITHUB_SHA" > build-version.tex
```

`tex/config/version-config.tex` automatically loads `build-version.tex` when it
exists. The generated file should not be committed.

## Frontpage

Frontpages are selected with project metadata:

```tex
\ProjectSetup{frontpage = default}
```

`default` is the initial value and intentionally does nothing. The alias `none`
also selects the no-op frontpage. This means documents can safely call:

```tex
\MakeProjectFrontPage
```

without producing a title page unless another frontpage is selected.

Add frontpage layouts as plain TeX files under `tex/frontpage/`, then register
them in `tex/frontpage/frontpage-config.tex`.
