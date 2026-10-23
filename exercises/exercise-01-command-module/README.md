# Exercise 01: Command Module

## Explanation

This playbook connects to every host in your inventory and does two things:

1. **check uptime**: runs the `uptime` command on the remote host using the `ansible.builtin.command` module. The result is saved in a variable called `command_output` using `register`.
2. **command output**: prints the saved output using the `ansible.builtin.debug` module. `stdout_lines` shows the output as a list of lines, which is easier to read than one long string.

## Definition

- **Playbook**: a YAML file that describes what Ansible should do on which hosts.
- **Play**: a block in the playbook that maps a group of hosts to a list of tasks (here, the whole file is one play).
- **hosts: all**: runs the play on every host in the inventory.
- **Task**: a single action, written as a name plus a module.
- **Module**: a built-in tool Ansible uses to do work (`command`, `debug`).
- **ansible.builtin.command**: runs a command on the remote host without a shell, so pipes (`|`), redirects (`>`) and variables like `$HOME` do not work.
- **register**: saves a task's result into a variable for later use.
- **ansible.builtin.debug**: prints a message or variable during the run.
- **stdout / stdout_lines**: the command's output as one string / as a list of lines.
- **changed_when**: lets you control when a task reports "changed".

## How to Run

```bash
ansible-playbook -i inventory.ini playbook.yml
```

## Expected Question

1. What does `register` do in this playbook?
2. Why do we use `command_output.stdout_lines` instead of printing `command_output` directly?
3. The `command` module normally reports `changed` every time it runs. How do you stop that for a read-only command like `uptime`?
4. Will `ansible.builtin.command: uptime | grep load` work? Why or why not?

## Answer

1. `register` stores the result of the `check uptime` task (output, return code, errors, etc.) in the variable `command_output`, so the next task can use it.
2. `command_output` contains a lot of extra data (`rc`, `start`, `end`, `stderr`, and so on). `stdout_lines` gives only the command's output, split into readable lines.
3. Add `changed_when: false` to the task. This tells Ansible the task never changes anything on the host.
4. No. The `command` module does not use a shell, so the `|` is not interpreted. Use `ansible.builtin.shell` instead when you need pipes or redirects.
