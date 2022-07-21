# ansible-cybera-postgres

Installs and configures Postfgres for use as in an IDSaaS environment

## Usage

### Variables

See the [defaults/main.yml](./defaults/main.yml) file for the variables
you can set in your Ansible playbooks.

You'll need to specify them in either in group_vars `group_vars/postgres.yml` (If you plan on having more than one Suricata host)

Or you can specify them in host_vars `host_vars/postgres_host_name.yml` (If you plan on having just the one Suricata host)

### Within a Playbook

This role can be used in a playbook like so:

```yaml
- name: Foo
  hosts: all
  tasks:
    - name: Apply Postgres role
      include_role:
        name: "ansible-cybera-postgres"
      become: yes
```

This will cause the `tasks/main.yml` file to trigger, which will subsequently
run all other plays in the `tasks` directory.

If you only want to run specific plays from the `tasks` directory, you will
need to do something like:

```yaml
- name: Foo
  hosts: all
  tasks:
    - name: Just configure Postgres
      include_role:
        name: "ansible-cybera-postgres"
        tasks_from: configure
      become: yes

```

## Requirements

Requirements are detailed in the accompanying requirements.yml file.
To install the requirements, ensure you are 'sourced' into the ansible venv and you pwd is at the root of this directory.

Run:

``` bash

ansible-galaxy -r requirements.yml

```

## Testing

Install Docker and Molecule:

```shell
# Docker
apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update -qq
apt-get install -y docker-ce docker-ce-cli containerd.io

# Molecule
python3 -m pip install "molecule[docker,lint]"

```

And then do:

```shell
$ mkdir work
$ cd work
$ git clone https://git.cybera.ca/cybera/ansible-cybera-postgres
$ cd ansible-cybera-postfgres
$ molecule test
```
