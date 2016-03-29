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
- Expand includes to be able to execute 'role tasks' in 'role context'.
```    
    - include: main.yml
        role: myrole

    - include: role=myrole other.yml
      vars:
         var1: val1
```
  This allows for a more controlled execution and easier reuse of roles and it's resources.
