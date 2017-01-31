### THIS WAS IMPLMENTENTED AS include_role

# Proposal: Revamp Roles

*Author*: Brian Coca <@bcoca>

*Date*: 04/01/2016

- Status: New
- Proposal type: core design
- Targeted Release: 2.2
- PR for Comments: 
- Estimated time to implement: 4 weeks


## Motivation
Roles are a good way to share and reuse resources, but they are a bit limited in application.
This proposal aims at expanding the usefulness and reusability of roles.

### Problems
Roles lack flexibility in execution and inclusion.

## Solution proposal
- Add exec_main flag to roles (default True to keep current behaviour), when false a role will not execute tasks/main.yml in `roles:` section of a play.
```
    - roles:
        - role: myrole
          exec_main: no
```
- Expand includes to be able to execute 'role tasks' in 'role context'. This will work within plays and within role tasks. The role's variables are added to the current context, the role's files are added to the search path, etc.
```    
    - include: main.yml
        role: myrole

    - include: role=myrole other.yml
      vars:
         var1: val1
```

- Allow the default of `exec_main` to be overridden by a role in its `meta/main.yml` so it doesn't have to be specified each time a role is invoked

- In `meta/main.yml` allow a role to depend on another with variable scope and file search path inheritance. Unlike specfying  an include via `exec_main: no` at the start of the role's execution, this sets the included roles variables before the including role's so the including role can reference the included role's variables in its defaults, etc. This also avoids the need to duplicate the same role import in all of a role's `tasks/` YML files. e.g.
```
  dependencies:
    - { role: postgres_server, inherit: true }
```

  This provides for a primitive form of inheritance, where one role can use another as a base and extend it. A role for a server with apache configured with ldap auth and a postgres database could inherit apache2, ldap and postgres roles then do the work required to configure them to talk to each other, and it'd be able to reference the other roles' tasks like `restart_apache.yml` when doing so. (Currently to do this you have to explicitly use paths to the other roles' tasks and will have issues with variable scoping/availability).

- To make this more convenient to use within playbooks, when a role is referenced in a play a non-default task may be specified for the role. This task is run after the role's `main.yml` if `exec_main` is true, otherwise instead of `main.yml`. The suffix `.yml` is added implicitly if not specified. e.g.
```
- hosts: somehosts
  roles:
    - { role: postgres_server, ansible_task: install }
```
  would include role bdrnodes, check its `meta/main.yml` for `exec_main`, run `roles/postgres_server/tasks/main.yml` if `exec_main` is not false, then run `roles/postgres_server/tasks/install.yml` . The `ansible_` prefix is used to try to avoid clashes with existing user variables named "task".

This allows for a more controlled execution and easier reuse of roles and it's resources.
