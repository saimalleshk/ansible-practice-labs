# ansible-practice-labs
Hands-on Ansible exercises with playbooks, explanations, and practice questions

# Ansible Practice Labs

Hands-on Ansible exercises for beginners. Each exercise has a working playbook and a short README with an explanation, key definitions, practice questions, and answers.

## Prerequisites

- Python 3.9 or newer
- Ansible installed (`pip install ansible`)
- At least one Linux machine you can reach over SSH (a VM, a cloud instance, or a Docker container)
- SSH key access to that machine

## Setup

1. Clone the repo:

   ```bash
   git clone https://github.com/<your-username>/ansible-practice-labs.git
   cd ansible-practice-labs
   ```

2. Edit `inventory.ini` and add your own hosts:

   ```ini
   [all]
   server1 ansible_host=192.168.1.10 ansible_user=ubuntu
   ```

3. Test the connection:

   ```bash
   ansible all -i inventory.ini -m ansible.builtin.ping
   ```

   You should see `pong` in green for each host.

## How to Use This Repo

1. Open an exercise folder and read its `README.md`.
2. Try to answer the **Expected Question** section yourself first.
3. Run the playbook from the repo root:

   ```bash
   ansible-playbook -i inventory.ini exercises/exercise-01-command-module/playbook.yml
   ```

4. Check your answers against the **Answer** section.

## Check Your Work

Run these linters before you commit any changes:

```bash
pip install ansible-lint yamllint
yamllint .
ansible-lint
```

## Exercises

| # | Topic | Concepts |
|---|-------|----------|
| 01 | [Command Module](exercises/exercise-01-command-module/) | `command`, `register`, `debug`, `changed_when` |

More exercises are added over time.

## Repo Structure

```
ansible-practice-labs/
├── README.md
├── inventory.ini
└── exercises/
    └── exercise-01-command-module/
        ├── README.md
        └── playbook.yml
```

## Contributing

Found a mistake or have an exercise idea? Open an issue or a pull request. Please run `ansible-lint` first.

## License

MIT
