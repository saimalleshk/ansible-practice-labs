# Ansible Practice Labs

Hands-on Ansible exercises for Linux. Each exercise has a working playbook and a short README with an explanation, key definitions, practice questions, and answers.

## Prerequisites

- A Linux machine (Ubuntu, Debian, Fedora, RHEL, Rocky, etc.) to run Ansible from
- Python 3.9 or newer (`python3 --version`)
- `sudo` access on your machine
- Optional: a second Linux machine or VM for remote practice

## Install Ansible

Ubuntu / Debian:

```bash
sudo apt update
sudo apt install -y ansible
```

Fedora / RHEL / Rocky / AlmaLinux:

```bash
sudo dnf install -y ansible-core
```

Verify:

```bash
ansible --version
```

## Setup

1. Clone the repo:

   ```bash
   git clone https://github.com/<your-username>/ansible-practice-labs.git
   cd ansible-practice-labs
   ```

2. Choose how you want to practice.

   **Option A: Run against your own machine (easiest)**

   `inventory.ini` is already set up for this. No SSH needed:

   ```ini
   [lab]
   localhost ansible_connection=local
   ```

   **Option B: Run against other Linux machines**

   Create an SSH key (skip if you already have one) and copy it to each machine:

   ```bash
   ssh-keygen -t ed25519
   ssh-copy-id ubuntu@192.168.1.10
   ```

   Then add the machines to `inventory.ini`:

   ```ini
   [lab]
   server1 ansible_host=192.168.1.10 ansible_user=ubuntu
   ```

3. Test the connection:

   ```bash
   ansible lab -i inventory.ini -m ansible.builtin.ping
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

```bash
sudo apt install -y ansible-lint yamllint     # Ubuntu / Debian
# or: pip install --user ansible-lint yamllint

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
├── .gitignore
└── exercises/
    └── exercise-01-command-module/
        ├── README.md
        └── playbook.yml
```

## Contributing

Found a mistake or have an exercise idea? Open an issue or a pull request. Please run `ansible-lint` first.

## License

MIT
