# IDE-Like Features in Vim

A lot of devs I know use [VSCode][vscode] - even former Vim users, who use it
with the [VSCodeVim][vscodevim] extension.

VSCode is what I'd recommend to any new dev getting started. It has broad
language support, [lots of extensions][marketplace] and is easy-to-use.
Microsoft also deserves kudos for creating the [language server protocol][lsp],
which shifted language support from the IDE developers to the compiler
developers, making it easier for _other_ editors to provide good language
support too.

But I'm a bit too stuck in my ways to move away from Vim, so I use a Vim plugin
called [Coc][coc] that acts as an LSP client. There are a lot of LSP clients
for Vim, but this one also supports [VSCode-like extensions][coc-extensions] -
so setting up Go language support (for example) is as easy as typing
`:CocInstall coc-go`.

## C/C++ Support

To get set up for C++ development, I needed to install a language server (using
`dnf install clangd`) and Coc extension (by running `:CocInstall coc-clangd` in
Vim). So this was similar to Go.

The Go build tool figures out how to link other modules for you, and the Go
language server (which is built by the Go developers) is able to use the same
logic to figure out how the code in your file relates to other code in standard
or third-party libraries.

But when you compile a C++ file with Clang or GCC, you have to tell your
compiler things like where to find the header files via the `-I` flag. Your
language server _also_ needs to be told where to find header files, so that it
can provide things like auto-completion and let you jump into library code.

Your build system usually invokes the compiler with these flags for you, and it
can probably be configured to generate a compilation database for you too. For
example with Buck, you can do this with:

```
buck build //example:example#compilation-database --out compile_commands.json
```

But a simple alternative (which only works for simple use cases) is to create a
file called `compile_flags.txt` which contains the compiler flags - which are assumed to be the same for every source file:

```
$ cat compile_flags.txt
-xc++
-I
/usr/include/octave-7.2.0/
```

More info on how to set up `compile_commands.json` or `compile_flags.txt` can
be found at: <https://clang.llvm.org/docs/JSONCompilationDatabase.html>.

[vscode]: https://code.visualstudio.com/
[vscodevim]: https://marketplace.visualstudio.com/items?itemName=vscodevim.vim
[marketplace]: https://marketplace.visualstudio.com/VSCode
[lsp]: https://microsoft.github.io/language-server-protocol/
[coc]: https://github.com/neoclide/coc.nvim/
[coc-extensions]: https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions
