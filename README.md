# FIlePaths.jl

[![Build Status](https://travis-ci.org/rofinn/FilePaths.jl.svg?branch=master)](https://travis-ci.org/rofinn/FilePaths.jl)
[![codecov.io](https://codecov.io/github/rofinn/FilePaths.jl/coverage.svg?branch=master)](https://codecov.io/rofinn/Rory-Finnegan/FilePaths.jl?branch=master)

FilePaths.jl provides a type based approach to working with filesystem paths in julia.

## Intallation:
FilePaths.jl is registered, so you can to use `Pkg.add` to install it.
```julia
julia> Pkg.add("FilePaths")
```

## Usage:
```julia
julia> using FilePaths
```

The first important difference about working with paths in FilePaths.jl is that a path is an immutable list (Tuple) of strings, rather than simple a string.

Path creation:
```julia
julia> Path("~/repos/FilePaths.jl/")
Paths.PosixPath(("~","repos","FilePaths.jl",""))
```
or
```julia
julia> p"~/repos/FilePaths.jl/"
Paths.PosixPath(("~","repos","FilePaths.jl",""))
```

Human readable file status info:
```julia
julia> stat(p"README.md")
Status(
  device = 16777220,
  inode = 48428965,
  mode = -rw-r--r--,
  nlink = 1,
  uid = 501,
  gid = 20,
  rdev = 0,
  size = 1880 (1.8K),
  blksize = 4096 (4.0K),
  blocks = 8,
  mtime = 2016-02-16T00:49:27,
  ctime = 2016-02-16T00:49:27,
)
```

Working with permissions:
```julia
julia> m = mode(p"README.md")
-rw-r--r--

julia> m - readable(:ALL)
--w-------

julia> m + executable(:ALL)
-rwxr-xr-x

julia> chmod(p"README.md", "+x")

julia> mode(p"README.md")
-rwxr-xr-x

julia> chmod(p"README.md", m)

julia> m = mode(p"README.md")
-rw-r--r--

julia> chmod(p"README.md", user=(READ+WRITE+EXEC), group=(READ+WRITE), other=READ)

julia> mode(p"README.md")
-rwxrw-r--

```


Reading and writing directly to file paths:
```julia
julia> write(p"testfile", "foobar")
6

julia> read(p"testfile")
"foobar"
```

All the standard methods for working with paths in base julia exist in the FilePaths.jl. The following describes the rough mapping of method names. Use `?` at the REPL to get the documentation and arguments as they may be different than the base implementations.

Base | FilePaths.jl
--- | ---
pwd() | cwd()
homedir() | home()
cd() | cd()
joinpath() | joinpath()
basename() | basename()
N/A | filename
N/A | extension
N/A | extensions
ispath | exists
realpath | real
normpath | norm
abspath | abs
relpath | relative
N/A | glob
stat | stat
lstat | lstat
filemode | mode
N/A | modified
N/A | created
isdir | isdir
isfile | isfile
islink | islink
issocket | issocket
isfifo | isfifo
ischardev | ischardev
isblockdev | isblockdev
isexecutable (deprecated) | isexecutable
iswritable (deprecated) | iswritabe
isreadable (deprecated) | isreadable
ismount | ismount
isabspath | isabs
N/A | drive
N/A | root
expanduser | expanduser
mkdir | mkdir
mkpath | N/A (use mkdir)
symlink | symlink
cp | copy
mv | move
rm | remove
touch | touch
tempname | tmpname
tempdir | tmpdir 
mktemp | mktmp 
mktempdir | mktmpdir 
chmod (non-recursive) | chmod (recursive unix-only)
chown (PR) | chown (unix only)
N/A | read
N/A | write

## TODO:
* cross platform chmod and chown


