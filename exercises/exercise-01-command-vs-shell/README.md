# Exercise 01: Command vs Shell

## Explanation

This exercise has three playbooks:

1. **`command-uptime.yml`**: runs `uptime` with the `command` module and prints the output. `register` saves the result, and `debug` prints `stdout_lines`.
2. **`command-glob.yml`**: runs `ls -l *` with the `command` module. This **fails** with an error like `ls: cannot access '*': No such file or directory`. The `command` module runs the program directly, with no shell, so `*` is never expanded. `ls` receives a literal `*` and looks for a file with that name.
3. **`shell-glob.yml`**: runs the same `ls -l *` with the `shell` module. This works, because the command runs through a shell (`/bin/sh`), which expands `*` into the list of files.

The only difference between 2 and 3 is the module name, and that is the point of the exercise.

## Definition

- **ansible.builtin.command**: runs a command directly on the host, without a shell. It is safer and more predictable, but pipes (`|`), redirects (`>`), wildcards (`*`) and variables like `$HOME` do not work.
- **ansible.builtin.shell**: runs a command through a shell. Pipes, redirects, wildcards and variables work, but it is more exposed to quoting mistakes and shell injection.
- **Glob / wildcard**: a pattern such as `*` that the shell expands into matching file names.
- **register**: saves a task's result into a variable.
- **stdout_lines / stderr_lines**: the normal output / error output, split into a list of lines.
- **changed_when: false**: tells Ansible the task never changes anything, so it never reports "changed".
- **ignore_errors**: lets the play continue after a failed task.

## How to Run

Run from the repo root:

```bash
ansible-playbook -i inventory.ini exercises/exercise-01-command-vs-shell/command-uptime.yml
ansible-playbook -i inventory.ini exercises/exercise-01-command-vs-shell/command-glob.yml
ansible-playbook -i inventory.ini exercises/exercise-01-command-vs-shell/shell-glob.yml
```

## Expected Question

1. Why does `ls -l *` fail with the `command` module but work with the `shell` module?
2. What does `command-glob.yml` print for `stderr_lines`, and why?
3. Why is `ignore_errors: true` used in `command-glob.yml`? What would happen without it?
4. Which module should you use by default, and why?
5. You need to run `ps aux | grep sshd`. Which module do you use?

## Answer

1. `command` runs the program directly, so `*` is passed to `ls` as a literal character. `shell` starts a shell first, and the shell expands `*` into file names before `ls` runs.
2. It prints an error such as `ls: cannot access '*': No such file or directory`, because no file is literally named `*` in that directory.
3. Without it, the failed task would stop the play, and the debug task would never run. With it, the play continues and we can see the error.
4. Use `command` by default. It is safer (no shell injection) and more predictable. Use `shell` only when you actually need shell features such as pipes, redirects or wildcards.
5. `shell`, because the `|` pipe needs a shell.

## Production Note

- Prefer `command` unless you need shell features. `ansible-lint` has a rule (`command-instead-of-shell`) that flags `shell` used without them.
- For listing or finding files, the `ansible.builtin.find` module is better than `ls`, because it returns structured data and works the same on every system.
- When `shell` has a pipe, add `set -o pipefail` and `executable: /bin/bash`, so a failure on the left side of the pipe is not hidden.
