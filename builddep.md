# Useful DNF Sub-Commands

Linux package managers (e.g. apt and dnf) let you install, update and remove
packages. This is as simple as running sub-commands like `dnf install` and `dnf
remove`, and most of the time this is all I use it for. Occasionally though, I
need to do other things.

## Installing Build Dependencies

For example, recently I was trying to build a package from source and wanted to
install the build-time dependencies. One way to do this is to read through the
documentation and install each dependency manually. But a quicker way (at least
to get started) is to use:

```
# dnf builddep octave
```

This will install all the build-time dependencies that are needed to build
Octave, including things like GNU C++ and GNU Fortran. These are not run-time
dependencies, so they won't get installed with `dnf install octave`.

# Rollback and Undo

If you're installing a single package with `dnf install`, you can remove it
with `dnf remove`, but there's no equivalent for `dnf builddep`. To remove
build dependencies, you can rollback or undo an item in the history.

The difference between a rollback and an undo is that rollback brings you back
to a point in history, whereas undo just undoes a specific entry. For example:

```
$ dnf history
ID     | Command line              | Date and time    | Action(s)      | Altered
--------------------------------------------------------------------------------
   123 | install clang clang-tools | 2022-12-06 17:31 | Install        |    2   
   122 | builddep octave-devel     | 2022-12-06 17:28 | Install        |  111   
...
```

Typing `dnf history undo 122` will uninstall all the build dependencies of
`octave-devel`, whereas typing `dnf history rollback 122` will _also_ uninstall
`clang` and `clang-tools`.

## Searching for Files and Packages

Sometimes I want to find out which package I need to install to get a specific
file. This can be done using:

```
$ dnf provides */liboctinterp.so
```

It's also possible to get a list of files that a specific package will install
(the inverse of the above):

```
$ dnf repoquery -l octave-devel
```
