This file contains temporary notes


# Workspace

- all code in a single workspace

- `GOPATH` is an env variable to show the system where this workspace is
    + it can be a list of paths, but usually it's just one
    + it defaults to `$HOME/go`
    + set it in `bash_profile`
    + add `$GOPATH/bin` to `$PATH`

- go tools installation:
    + use package manager
    + download archive from https://golang.org/doc/install, then unpack it under `/usr/local/go`
        and add `/usr/local/go/bin` to `$PATH`

- directory organization
    + `$GOPATH/src`: root for source code (new one, as well as downloaded). It determines the
        import path.
    + `$GOPATH/pkg/$GOOS_GOARCH`: root for a package binary
    + `$GOPATH/bin`: root for a command binary (e.g. `$GOPATH/src/A/B` source has binary in
        `$GOPATH/bin/B`). This one one just needs to add `$GOPATH/bin` to `$PATH` to have all
        executables visible.
    + **internal** named directories - inside a package - are only visible by source code
        rooted at (the same) package level.
    + **vendor** named directories have the same visibility of **internal**, in plus the import
        path is as they are package at src level. E.g. while internal code is imported as
        `foo/internal/pkg`, code in vendor subtree is imported with `pkg`, not `foo/vendor/pkg`.
        This way - under the vendor subtree - it is possible to store custom (or vendor) copy
        of some external dependencies. `go get` updates also submodules.
        The custom vendor package is imported only by the source code rooted at the same package root
        as the vendor directory (i.e. only under `foo`), while the other packages will import the
        version under `src` directory.

- go tools prevent overriding default packages: e.g. a `$GOPATH/src/cmd/mycmd...` project
  won't be built because `cmd` is a default package. Basically the check is done looking into
  `GOROOT` first, it includes some directories, a `cmd` named one as well, so it will skip
  any `cmd` directory inside `GOPATH`

Open points: GOOS, GOARCH, GOROOT

