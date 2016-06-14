# Separate Extras Modules from Ansible Core, Move it to Community.

*Author*: Jason McKerr <@newtMcKerr>, James Cammarata

- Date: 13 June, 2016

- Status: New

- Proposal Type: Community Development Process.

- Targeted Release: 2.2 or possibly 2.3

- Estimated time to Implement: 3-4 months.

## Motivation and Problems
There are currently two module repositories: ansible-modules-core, and ansible-modules-extras.  Traditionally this has been recognized to mean that there is a standard for Core modules (since they are to be supported by the Ansible Core team and Red Hat) and Extras (modules that are part of Ansible, but more broadly contributed to and less supported).  In practice, Extras became roughly the same thing as Core since Extras are both distributed with Ansible and supported by the Ansible Core team.  

That condition had the unintended consequences of forcing the Core team to set the requirements for Extras modules to be roughly the same as Core.  The result was that lots of modules contributed by the Ansible community were either rejected or asked to reach a certain level of robustness or feature-completion for inclusion.  This gave the community little room to have "starter" functionality, or a place to contribute simpler or differentiated functionality that is not necessarily a part of the "Core" product, but is nonetheless a good place to get other functionality into ansible.

## Solution and Approach
The Ansible Core team and the Ansible Community team have decided to split Extras (ansible-modules-extras) from the Ansible Core and Ansible Modules Core project. Modules will still use the Ansible community process, but will now be managed by the Ansible Community Team and the Ansible Community in general while the Core team will focus on the Core Modules.

  - This means that ansible-modules-extras will have it's own documentation and process for contributing modules to Extras. 
  - It also means that EXTRAS WILL NO LONGER BE PART OF THE STANDARD ANSIBLE INSTALL.

### Things that need to happen
  - An audit of the Extras and Core modules to ensure that all modules are in the correct repository (Extras or Core).
  - Ensure that modules in a repository meet that repositories guidelines, or at least note it for future work.
  - Have different process and guideline documentation for the Core Modules and Extras Modules contributions and code.
  - Build release processes for both.
  - Define release scheduling so that versions do not become conflicting.
  - Decide and implement stragey for module_utils
  - Document a progression path for moving modules from Extras to Core (or vice versa)

## Conclusion
  - This needs a lot of feedback on the how's and when's of it.  Please weigh in!
