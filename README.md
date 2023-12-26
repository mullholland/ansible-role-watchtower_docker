# [Ansible role watchtower_docker](#watchtower_docker)

Installs and configures watchtower container based on official watchtower docker container

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-watchtower_docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-watchtower_docker/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/watchtower_docker)](https://galaxy.ansible.com/mullholland/watchtower_docker)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-watchtower_docker.svg)](https://github.com/mullholland/ansible-role-watchtower_docker/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-watchtower_docker/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  roles:
    - role: "mullholland.watchtower_docker"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-watchtower_docker/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true
  vars:
    pip_packages:
      - "docker"

  roles:
    - role: mullholland.docker
    - role: mullholland.repository_epel
    - role: mullholland.pip
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-watchtower_docker/blob/master/defaults/main.yml):

```yaml
---
# General config
watchtower_docker_network_name: "web"
watchtower_docker_base_path: "/opt"
watchtower_docker_timezone: "Europe/Berlin"

# User/Group of the stack. Everything is mapped to this, instead of root.
watchtower_docker_user: "homelab"
watchtower_docker_uid: "900"
watchtower_docker_group: "homelab"
watchtower_docker_gid: "900"
watchtower_docker_user_system: true

# which container version to install
# can also be latest
watchtower_docker_version: "containrrr/watchtower:latest"

# additional docker compose environment variables
# https://containrrr.dev/watchtower/arguments/
watchtower_docker_environment_variables:
  - "WATCHTOWER_CLEANUP: true"             # Removes old images after updating
  - "WATCHTOWER_INCLUDE_RESTARTING: true"  # Will also include restarting containers.
  - "WATCHTOWER_ROLLING_RESTART: true"     # Restart one image at time instead of stopping and starting all at once
  - "WATCHTOWER_POLL_INTERVAL: 3600"       # check every hour for image updates
  # - "WATCHTOWER_LABEL_ENABLE: true"      # Monitor and update containers that have a com.centurylinklabs.watchtower.enable label set to true, else update all
  # Watchtower - Notifications
  - "WATCHTOWER_LIFECYCLE_HOOKS: true"     # to allow managing of lifecycle hooks via labels (https://containrrr.dev/watchtower/lifecycle-hooks/#executing_commands_before_and_after_updating)
  # - "WATCHTOWER_NOTIFICATION_REPORT: true"
  # - "WATCHTOWER_NOTIFICATION_URL: discord://token@channel"
  # metrics
  - "WATCHTOWER_HTTP_API_METRICS: true"    # Enables a metrics endpoint
  - 'WATCHTOWER_HTTP_API_TOKEN: SuperSecretToken'
watchtower_docker_volumes:
  - "/var/run/docker.sock:/var/run/docker.sock:ro"

# which port to expose. can be empty if used with watchtower for example
watchtower_docker_ports: []
#  - "8080:8080"
watchtower_docker_labels:
  - "traefik.enable=false"

```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-watchtower_docker/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.repository_epel](https://galaxy.ansible.com/mullholland/repository_epel)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-repository_epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-repository_epel/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-repository_epel)|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-docker)|
|[mullholland.pip](https://galaxy.ansible.com/mullholland/pip)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-pip/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-pip/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-pip)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-watchtower_docker/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|38, 39|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-watchtower_docker/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-watchtower_docker/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
