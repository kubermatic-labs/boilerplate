# boilerplate

`boilerplate` is a Go-based tool to verify that a given set of files
or directories contain a predefined boilerplate/license header comment.
The expected boilerplate is configured based on the file extensions.

The implementation is based on a Go-rewrite of Kubernetes'
[boilerplate](https://github.com/kubernetes/kubernetes/tree/master/hack/boilerplate)
Python script.

## Installation

```
go get github.com/kubermatic-labs/boilerplate
```

## Usage

Create a directory to store your boilerplate files. For every filetype
you scan, create a `boilerplate.{EXTENSION}.txt` file, for example
`boilerplate.go.txt` for all `*.go` files.

Then simply run `boilerplate` with the directory you just created and
1 or more sources (files or directories) to scan:

```bash
./boilerplate -boilerplates hack/myboilerplates/ pkg cmd
```

The command above would scan the `pkg` and `cmd` directories.

`boilerplate` exits with code 0 when no errors were found, 1 otherwise.

You can exclude paths by using `-exclude` (can be used multiple times).
Exclude expressions are glob expressions rooted at the current working
directory when starting `boilerplate`:

```bash
./boilerplate \
  -boilerplates hack/myboilerplates/ \
  -exclude pkg/internal/ \
  -exclude 'cmd/internal-*' \ # make sure to properly quote to prevent shell globbing
  pkg cmd
```

The following expressions are hardcoded to always be excluded:

* `.git`
* `vendor`
* `_build`
* `zz_generated.*`
* `zz_generated_*`
