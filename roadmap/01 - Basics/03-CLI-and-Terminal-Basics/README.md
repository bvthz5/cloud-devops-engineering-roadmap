# CLI and Terminal Basics

Welcome to the definitive guide on **Command-Line Interface (CLI) and Terminal Basics** for Cloud, DevOps, and Platform Engineers. The terminal is the primary cockpit for managing remote servers, Kubernetes clusters, CI/CD runners, and cloud infrastructure.

---

## Complete Topic Syllabus & Roadmap Mapping

Below is the complete syllabus mapping all **90 fundamental topics** plus **modern CLI productivity tools and defensive shell scripting practices**:

### 1. Terminal Architecture & Shell Flavors (Topics 1–9)
1. Command-Line Interface (CLI)
2. Terminal (TTY, physical vs virtual)
3. Terminal Emulator (Alacritty, iTerm2, WezTerm, Windows Terminal, tmux)
4. Shell (Command interpreter)
5. Shell Types (Login vs Non-login, Interactive vs Non-interactive)
6. Bash (Bourne-Again Shell - Linux standard)
7. Zsh (Z Shell - macOS default, Oh My Zsh)
8. Fish (Friendly Interactive Shell - autocompletion)
9. PowerShell (Object-oriented shell, Windows & cross-platform)

### 2. Syntax, Paths, & Navigation (Topics 10–23)
10. Command
11. Command Syntax (`command [options] [arguments]`)
12. Command Options (Short options `-a`, combined `-la`)
13. Command Arguments (Operands)
14. Command Flags (Long options `--all`)
15. Command Path
16. Absolute Path (Starts at `/`)
17. Relative Path (Starts at working directory)
18. Current Directory (`pwd`)
19. Parent Directory
20. Home Directory (`$HOME`)
21. Root Directory (`/`)
22. `.` (current) and `..` (parent)
23. `~` (tilde expansion to user home)

### 3. Execution, Resolution, & Built-ins (Topics 24–29, 74–78)
24. PATH Environment Variable (`$PATH` lookup order)
25. Command Resolution (Alias → Function → Built-in → PATH external)
26. Built-in Commands (`cd`, `pwd`, `echo`, `type`, `kill`)
27. External Commands (Binaries in `/bin`, `/usr/bin`)
28. Shell Functions
29. Aliases (`alias`, `unalias`)
74. `which` (Locates executable binary in PATH)
75. `whereis` (Locates binary, source, and manual pages)
76. `type` (Reveals command classification)
77. `command` (Bypasses aliases and functions)
78. `alias`

### 4. Variables, Expansion, & Output (Topics 30–37, 79–82, 88)
30. Environment Variables (`export`)
31. Shell Variables (Local to shell process)
32. Exported Variables (Inherited by child processes)
33. Command Substitution (`$(command)` and backticks)
34. Variable Expansion (`$VAR`, `${VAR}`)
35. Tilde Expansion (`~`, `~user`)
36. Parameter Expansion (`${VAR:-default}`, `${VAR//find/replace}`)
37. Filename Expansion (Pathname generation)
79. `env`
80. `printenv`
81. `echo`
82. `printf` (Format specifiers `%s`, `%d`, `%10s`)
88. Command Substitution (Nesting, differences between backticks and `$()`)

### 5. Wildcards, Globbing, & Quoting (Topics 38–43)
38. Wildcards (`*`, `?`, `[]`)
39. Globbing (Path expansion, extended globbing `shopt -s extglob`)
40. Quoting Mechanics
41. Single Quotes (`'...'` - Strict literal preservation)
42. Double Quotes (`"..."` - Variable expansion permitted, literal spaces)
43. Escape Characters (`\` backslash escaping)

### 6. Streams, Redirection, & Pipes (Topics 44–51)
44. Standard Input (`stdin` - FD 0)
45. Standard Output (`stdout` - FD 1)
46. Standard Error (`stderr` - FD 2)
47. Pipes (`|` - Unix pipeline connecting stdout to stdin)
48. Output Redirection (`>`, `>>`)
49. Input Redirection (`<`, `<<EOF`, `<<<`)
50. Error Redirection (`2>`, `2>>`)
51. File Descriptor Redirection (`2>&1`, `&>`, `exec 3>&1`)

### 7. Chaining, Exit Codes, & Job Control (Topics 52–61)
52. Command Chaining
53. Sequential Execution (`;`)
54. Logical AND (`&&` - Runs if previous succeeded)
55. Logical OR (`||` - Runs if previous failed)
56. Exit Status (`$?`)
57. Exit Codes (0 = Success, 1 = General error, 127 = Not found, 137 = OOM/SIGKILL)
58. Process Execution
59. Foreground Processes
60. Background Processes (`&`, `nohup`)
61. Job Control (`jobs`, `fg`, `bg`, `disown`, `Ctrl+Z`)

### 8. History, Prompt, & Startup Configs (Topics 62–70)
62. Command History (`history`, `!`, `!!`, `!$`)
63. History Expansion
64. Tab Completion (Readline, bash-completion)
65. Shell Prompt (`PS1`, custom prompts, Starship)
66. Shell Configuration
67. `.bashrc` (Interactive non-login configuration)
68. `.bash_profile` (Interactive login configuration)
69. `.profile` (POSIX general login configuration)
70. `.zshrc` (Zsh configuration)

### 9. Documentation & Help Systems (Topics 71–73)
71. `man` (Manual pages, sections 1–8)
72. `info` (GNU info system)
73. `--help` (Built-in executable help flags)

### 10. Text Processing & Data Wrangling (Topics 83–87)
83. CLI Text Processing
84. CLI Search (`grep`, `egrep`, `grep -E`)
85. CLI Filtering (`cut`, `awk`, `sed`)
86. CLI Sorting (`sort`, `uniq -c`)
87. CLI Pipelines (Combining tools for data analysis)

### 11. Scripting Foundations & Troubleshooting (Topics 89–90)
89. Shell Scripting Fundamentals (Shebang `#!/usr/bin/env bash`, arguments `$1`, `$@`, `$#`, conditionals, loops)
90. CLI Troubleshooting (Debugging flags `set -x`, tracing, subshell isolation)

### 12. Modern DevOps CLI Extensions (Essential Additions)
- **Strict Mode in Bash:** `set -euo pipefail` (Crucial for CI/CD!)
- **Modern CLI Tools:** `ripgrep` (`rg`), `fd`, `bat`, `eza`, `fzf`, `jq`

---

## File Structure in this Module

- [`01-Terminal-vs-Shell-and-Shell-Flavors.md`](./01-Terminal-vs-Shell-and-Shell-Flavors.md)
- [`02-Command-Syntax-Paths-and-Directory-Navigation.md`](./02-Command-Syntax-Paths-and-Directory-Navigation.md)
- [`03-PATH-Command-Resolution-Builtins-and-Aliases.md`](./03-PATH-Command-Resolution-Builtins-and-Aliases.md)
- [`04-Variables-Environment-and-Shell-Expansions.md`](./04-Variables-Environment-and-Shell-Expansions.md)
- [`05-Wildcards-Globbing-and-Quoting-Mechanics.md`](./05-Wildcards-Globbing-and-Quoting-Mechanics.md)
- [`06-Streams-Redirection-Pipes-and-FDs.md`](./06-Streams-Redirection-Pipes-and-FDs.md)
- [`07-Command-Chaining-Exit-Codes-and-Job-Control.md`](./07-Command-Chaining-Exit-Codes-and-Job-Control.md)
- [`08-History-Completion-Prompt-and-Shell-Config-Files.md`](./08-History-Completion-Prompt-and-Shell-Config-Files.md)
- [`09-Help-Systems-and-Introspection-Tools.md`](./09-Help-Systems-and-Introspection-Tools.md)
- [`10-Text-Processing-Pipelines-and-Data-Wrangling.md`](./10-Text-Processing-Pipelines-and-Data-Wrangling.md)
- [`11-Shell-Scripting-Foundations-and-Defensive-Bash.md`](./11-Shell-Scripting-Foundations-and-Defensive-Bash.md)
- [`12-Modern-CLI-Replacements-and-Productivity-Tools.md`](./12-Modern-CLI-Replacements-and-Productivity-Tools.md)
- [`13-Real-World-Scenarios.md`](./13-Real-World-Scenarios.md)
- [`14-Troubleshooting.md`](./14-Troubleshooting.md)
- [`15-Interview-QA.md`](./15-Interview-QA.md)
- [`16-Hands-On-Practice.md`](./16-Hands-On-Practice.md)
- [`17-MCQ.md`](./17-MCQ.md)
- [`18-Quick-Revision.md`](./18-Quick-Revision.md)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| - | You are here | [01 - Terminal vs Shell and Shell Flavors](./01-Terminal-vs-Shell-and-Shell-Flavors.md) |
