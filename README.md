# satellite_repository

Ansible role for enabling, disabling, and synchronizing repositories with Satellite 6 using hammer.

## Role Variables

| parameter           | required | default | choices         | comments 
| ------------------- |:--------:|:-------:| --------------- |:-------- 
| `state`             | yes      | exists  | enable, disable | 
| `synchronize`       | no       | False   | True, False     | 
| `synchronize_async` | no       | True    | True, False     | 
| `organization`      | yes      |         |                 | 
| `product`           | yes      |         |                 | 
| `repository`        | yes      |         |                 | 
| `basearch`          | no       |         |                 | 
                                   
## Assumptions

Assumptions made by this Ansible role.

### `hammer` Command

This Ansible role assumes that the `hammer` command is installed and configured on the host this role is being run against.

## Example Playbooks

### Enable Satellite 6 Repositories

```
- name: Enable Satellite 6 Repositories Example
  hosts: localhost
  roles:
   - role: satellite_repository
      state: enable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server - Extras"
      basearch: "x86_64"

    - role: satellite_repository
      state: enable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server"
      basearch: "x86_64"
      release: "7Server"

```

### Disable Satellite 6 Repositories

```
- name: Disable Satellite 6 Repositories Example
  hosts: localhost
  roles:
   - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server - Extras"
      basearch: "x86_64"

    - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server"
      basearch: "x86_64"
      release: "7Server"
```

### Enable and Synchronously Syncronize Satellite 6 Repositories

```
- name: Enable and Synchronously Syncronize Satellite 6 Repositories Example
  hosts: localhost
  roles:
   - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server - Extras"
      basearch: "x86_64"
      synchronize: True
      synchronize_async: False

    - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server"
      basearch: "x86_64"
      release: "7Server"
      synchronize: True
      synchronize_async: False
```

### Enable and Asynchronously Syncronize Satellite 6 Repositories

```
- name: Enable and Asynchronously Syncronize Satellite 6 Repositories Example
  hosts: localhost
  roles:
   - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server - Extras"
      basearch: "x86_64"
      synchronize: True
      synchronize_async: True

    - role: satellite_repository
      state: disable
      organization: "{{ org }}"
      product: "Red Hat Enterprise Linux Server"
      repository: "Red Hat Enterprise Linux 7 Server"
      basearch: "x86_64"
      release: "7Server"
      synchronize: True
      synchronize_async: True
  post_tasks:
    - name: Wait for Repository syncronizations
      command: >
        hammer task progress \
          --id {{ item }}
      with_items: "{{ satellite_repository_syncronization_tasks }}"
```
