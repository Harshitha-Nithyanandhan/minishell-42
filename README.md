# minishell

> *As beautiful as a shell*

A custom Unix shell built from scratch in C, developed as part of the 42 school curriculum. Minishell replicates core bash behaviour — from input parsing and tokenization all the way through execution — giving a ground-up understanding of how shells actually work.

---

## About

Shells have been the primary interface between humans and computers since the earliest days of computing. This project rebuilds one from first principles: no shortcuts, no system calls beyond what the subject allows, and strict memory management throughout.

Built in collaboration with [BenGallagh42](https://github.com/BenGallagh42).

---

## Features

### Core shell behaviour
- Interactive prompt with command history
- Executable resolution via `PATH`, relative paths, and absolute paths
- Single global variable constraint (signal number only)

### Parsing pipeline
The input goes through a full **lexer → parser → executor** pipeline:
- **Lexer** — tokenizes input, handles dollar expansion, quotes, operators, redirectors, and wildcards
- **Parser** — builds command structures, handles heredoc, syntax validation, and expansion
- **Executor** — resolves paths, forks processes, handles redirections, manages pipes

### Quote handling
- `'single quotes'` — prevents all metacharacter interpretation
- `"double quotes"` — prevents metacharacter interpretation except for `$`

### Redirections
| Operator | Behaviour |
|----------|-----------|
| `<`  | Redirect stdin |
| `>`  | Redirect stdout |
| `>>` | Redirect stdout (append mode) |
| `<<` | Heredoc — read input until delimiter |

### Pipes
Chains commands so the output of each becomes the input of the next via `|`.

### Environment variables
- `$VARIABLE` expands to its value
- `$?` expands to the exit status of the last foreground pipeline

### Signal handling
| Signal | Behaviour |
|--------|-----------|
| `Ctrl-C` | Display a new prompt on a new line |
| `Ctrl-D` | Exit the shell |
| `Ctrl-\` | No action |

### Built-in commands
| Command | Description |
|---------|-------------|
| `echo [-n]` | Print text to stdout |
| `cd [path]` | Change directory |
| `pwd` | Print working directory |
| `export` | Set environment variables |
| `unset` | Unset environment variables |
| `env` | Print environment |
| `exit` | Exit the shell |

---

## Project structure

```
minishell/
├── inc/
│   └── minishell.h
├── src/
│   ├── main.c
│   ├── shell_innit.c
│   ├── shell_run.c
│   ├── shell_signals.c
│   ├── shell_free.c
│   ├── lexer/          # Tokenization, dollar/quote/wildcard handling
│   ├── parser/         # AST building, expansion, heredoc, syntax checks
│   ├── exec/           # Process forking, path resolution, redirections
│   └── built-ins/      # echo, cd, pwd, export, unset, env, exit
└── libft/              # Custom C standard library
```

### Execution pipeline

```
Input → Lexer → Tokens → Parser → Command tree → Executor → Output
                  ↓                     ↓
            Quote handling         Variable & wildcard expansion
            Dollar expansion       Heredoc processing
            Operator detection     Redirection setup
```

---

## Usage

```bash
make
./minishell
```

```bash
minishell$ echo "Hello, $USER!"
Hello, harshitha!

minishell$ ls | grep .c | wc -l
42

minishell$ cat << EOF
> Hello
> World
> EOF

minishell$ export MY_VAR=hello && echo $MY_VAR
hello

minishell$ exit
```

---

## Getting started

### Requirements
- `gcc`
- `make`
- `readline` library

### Build

```bash
git clone https://github.com/Harshitha-Nithyanandhan/minishell.git
cd minishell
make
```

### Run

```bash
./minishell
```

---

## What I learned

Building a shell from scratch is one of the most comprehensive systems programming exercises there is. This project required deep engagement with:

- **Process management** — `fork`, `execve`, `waitpid`, process trees
- **File descriptors** — `dup2`, `pipe`, managing stdin/stdout/stderr across processes
- **Signal handling** — `sigaction`, safe signal handler design with minimal global state
- **Lexing and parsing** — tokenizing ambiguous input, building command structures from a token stream
- **Memory management** — zero leaks under Valgrind across all execution paths

---

## 42 school

This project is part of the [42](https://42.fr) common core curriculum (Rank 4). 42 is a peer-to-peer programming school with no teachers or lectures — everything is project-based and evaluated by other students.
