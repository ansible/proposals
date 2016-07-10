# Proposal: Load included role vars files from defaults folder

*Author*: Jeff Geerling <@geerlingguy>

*Date*: 2016/07/09

- Status: New
- Proposal type: Ansible core variables change
- Targeted Release: 2.2
- Associated PR: TODO
- Estimated time to implement: TODO

## Motivation

Currently, [include_vars](http://docs.ansible.com/include_vars_module.html) will include variables files from within a role's vars directory automatically. It would be beneficial to multi-platform or more complex roles to also search for include files within the `defaults` directory, so certain variables could be overridden by playbooks using the roles.

### Problems

- Ansible only looks for the included vars files inside the role's `vars` directory. Included files inside `defaults` are ignored.
- Roles providing platform-specific variables (e.g. `Debian.yml` and `RedHat.yml` inside a `vars` folder) have a difficult time providing these defaults while allowing downstream playbooks to override the variables.
- Many roles (e.g. [`geerlingguy.java`](https://galaxy.ansible.com/geerlingguy/java/)) use a hack whereby a platform-specific variable is prefixed with `__` and then `set_fact` is used in a role task to build the variable if it's not defined already ([example](https://github.com/geerlingguy/ansible-role-java/blob/1.4.0/tasks/main.yml#L10-L13)).

## Solution proposal
- Ansible should look for the included vars files inside the role's `vars` directory, and also inside the role's `defaults` directory.
- Included defaults would then be easy to override by playbooks following Ansible's existing [Variable Precedence](http://docs.ansible.com/ansible/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable).

## Dependencies (optional)

N/A

## Testing (optional)

Create a role with two tasks inside `tasks/main.yml`:

```yaml
- name: Include OS-specific variables.
  include_vars: "{{ ansible_os_family }}.yml"

- name: Print a variable value for testing.
  debug: var=myrole_test_var
```

Inside the role's `defaults` folder, create two files:

```
example_role/
  defaults/
    Debian.yml
    RedHat.yml
```

Inside each, put a single variable:

```yaml
---
myrole_test_var: [put 'Debian' in Debian.yml, 'RedHat' in RedHat.yml]
```

Then create a playbook that uses this role:

```yaml
---
- hosts: all
  roles:
    - example_role
```

Run the role, once on a Debian or Ubuntu server, and once on a RedHat or CentOS server. If this is implemented correctly, you will see the debug task print out the respective OS family. If it is not working, you'll get an error (current state).

To make sure playbook role default variable overrides work, change the playbook to:

```yaml
---
- hosts: all
  vars:
    myrole_test_var: 'testing!'
  roles:
    - example_role
```

Then run the playbook again. This time, the debug task should print 'testing!'.

## Documentation (optional)

Documentation for [Role default variables](https://docs.ansible.com/ansible/playbooks_roles.html#role-default-variables) should probably be updated to include an example of this type of included default variable usage.

## Anything else?

N/A
