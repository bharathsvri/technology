# JShell — Java REPL

**JShell** (`jdk.jshell`) is an interactive **Read-Eval-Print Loop** for Java, useful for learning APIs, prototyping snippets, and validating expressions without a full `main` class.

## Launch

```bash
jshell
```

Exit with `/exit`.

## Essential Commands
- `/help` — list commands and syntax.
- `/vars`, `/methods`, `/types` — show declared symbols.
- `/edit` — open editor (environment-dependent).
- `/save file.jsh` — save session; `/open file.jsh` — load.
- `/classpath path` or `/cp` — add JARs or directories for dependencies.
- `/env` — show module and compiler settings.

## Semantics Highlights
- **Automatic variable creation** for expression results (`$1`, `$2`, …) unless assigned.
- **Forward references** allowed in some cases; errors reported when incomplete.
- **Semicolons** optional for simple snippets (JShell adds them where valid).

## Example Session

```text
jshell> record Point(int x, int y) {}
jshell> var p = new Point(3, 4)
p ==> Point[x=3, y=4]
jshell> p.x()
$3 ==> 3
```

## When to Use JShell vs IDE Scratch
- **JShell**: ultra-fast checks, teaching, verifying `java.time` math.
- **IDE scratch / unit tests**: multi-file refactors, debugger, CI alignment.

## Related Topics
- `104_Java_Versions_New_Features.md` (JShell introduced in Java 9)
- `01_Java_Basics.md`
