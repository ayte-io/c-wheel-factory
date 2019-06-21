# Wheel Factory

This is just a placeholder repo for future library.

Wheel factory is a thing for those who likes reinventing wheels -
creating CLI applications from scratch. It all started with absence of
good CLI frameworks in Java, so i thought it would be cool to implement 
one-size-fits-all C library.

## Features

The forthcoming project is implied to have those features:

### Namespaces and commands

All commands reside in namespaces, including root empty namespace.
Command `builder dependency tree visualize` should decode following 
tree:

```
<root>: namespace
  dependency: namespace
    tree: namespace
      visualize: command
```

#### Commands

Commands have handlers attached to them, which receive two parameters:

- Parsed configuration
- void*, which is implied to contain user-provided context (if any).

#### Namespace options

Namespace may declare it's own options. It is quite common to have 
option for all commands at once, e.g. `-p` for profile:

```
application -p staging serve
```

At the same time, library should allow end user to specify same-named
options for other namespaces and commands. Following command:

```
application -p staging serve -p 8080
```

Should produce following tree:

```
<root>:
  options:
    -p: staging
  
```

At the same time, if serve command doesn't have `-p` configured, it 
should be propagated to upper level, then, if nothing has been found on 
upper level, to lower level, i.e. library should recognize both 
examples:

```
# serve doesn't have -p configured
application serve -p staging

# root namespace doesn't have -p configured
application -p 8080 serve
```

#### Namespace arguments

Namespace may declare arguments as well. Following command:

```
steward release 0.1.0 push
```

should recognize 0.1.0 as argument.

At current moment it seems reasonable that namespace must have a fixed 
number of arguments.

### Arguments & Options

#### Argument arity

Library should support argument arity, except for ambiguous cases. E.g.
it should be safe to declare `executable command {arg1 x 2} {arg2 x 2}`, 
but `executable command`

### Option decoding

End user should be able to enable or disable following options:

- Decoding `-tulpn` in same way as `-t -u -l -p -n` 
- Decoding `-t4` and `-tttt` as `-t 4`
- Combination: decoding `-at4` as `-a -t 4`
- Decoding multi-symbol shortcut options: `-at4` can be `-at 4` if such
option exists

#### Global overrides

End user should be able to supply global overrides that take precedence
over command handlers

#### Custom validations

It should be possible for end user to provide custom validators for 
options and arguments.

### Predefined functionality

#### Help command

Good old help command that can be used with `-h` or `--help` flag or
with `help <command>`.