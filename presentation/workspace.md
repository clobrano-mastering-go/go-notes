---
title:
- GO workspace
theme:
- Copenhagen
---

# Where the all the code is stored

All code in a single workspace
(at least till go modules...)

# What is GOPATH

- a "list" of path where go looks for source
- defaults to `$HOME/go`
- set it in `bash_profile`


# Directory organization 1/

- `$GOPATH/src`: source code organized by VCS services (github, gitlab, ...)
- `$GOPATH/pkg/$GOOS_$GOARCH`: contains packages binaries
- `$GOPATH/bin`: contains command binaries

        e.g.
            source $GOPATH/src/github/myapp/<all sources>
            executable $GOPATH/bin/myapp

# Directory organization 2/

## internal

Directories named **internal** are visible only to source
code rooted at the same package level.


    e.g.
        [1] src/github/outerpkg
        [2] src/github/mypkg/internal/outerpkg
        [3] src/github/mypkg/source.go
            import mypkg/internal/outerpkg  # import [2]
            *import outerpkg does not work 
        [4] src/github/somepkg/source.go
            import outerpkg  # import [3]


# Directory organization 3/

## vendor

Similar to **internal**, however the call `import outerpkg`
works also inside `mypkg`.

Used for including custom (or *vendor*) versions of a package.

Vendor `outerpkg` is imported only by the source rooted at
the same package level as vendor `outerpkg` is.


# Module proxy protocol

## general
- GOPROXY env variable
- control the source of downloaded modules:
    - undefined or 'default': packages are downloaded from VCS service
    - off: prevent module downloads
    - URL: packages are downloaded from the proxy module webserver at the given URL

## proxy module
- a web service able to respond to GET requests with a proper (simple) protocol.


# Import paths

- identify univocally the package
- path can be absolute, relative and remote (e.g. related to github)
- VCS type is identified by the extension
    - whatever/mypackage**.bzr**
    - github/mypackage**.git**

# Modules (go v1.11)

- collection of co-related go packages
- source code doesn't need to stay in the same workspace
- will replace $GOPATH-based approach
