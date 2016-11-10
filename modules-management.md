# Proposal: Proposals - Change how we manage modules in Ansible (the product) and the Ansible Community

 - Date: 09 AUGUST, 2016

 - Status: Rewritten from June proposal

 - Proposal Type: Community Development Process.

 - Targeted Release: 2.3

 - Estimated time to Implement: 3-4 months.

## Motivation and Problems
There are currently two module repositories: ansible-modules-core, and ansible-modules-extras.  Traditionally this has been recognized to mean that there is a standard for Core modules (since they are to be supported by the Ansible Core team and Red Hat) and Extras (modules that are part of Ansible, but more broadly contributed to and less supported).  In practice, Extras became roughly the same thing as Core since both Core and Extras are distributed with Ansible and supported by the Ansible Core team.  

That condition had the unintended consequences of forcing the Core team to set the requirements for Extras modules to be roughly the same as Core.  The result was that lots of modules contributed by the Ansible community were either rejected or asked to reach a certain level of robustness or feature-completion for inclusion.  This gave the community little room to have "starter" functionality, or a place to contribute simpler or differentiated functionality that is not necessarily a part of the "Core" product, but is nonetheless a good place to get other functionality into ansible.

Having multiple repos also made it more difficult for contributors to contribute things besides the module itself.  module_utils and tests for modules would have to be contributed to the main ansible repository even though the module itself lived in the ansible-modules-extras repository.  Bugs for modules might end up submitted against the main ansible repository or the ansible-modules-core repository even though the module lived in ansible-modules-extras.

## Solution
Ansible Core team and Contributors (from the Contributor Summit at the 2016 Ansible Fest San Francisco) propose to create categories of ansible Modules where Modules are grouped by a rough category of like-functionality (Nagios Modules would be grouped together, as would AWS, VMware, etc).  There are a large number of implications for that working in the Ansible workflow and Community:
  - There *will not* be multiple repos.  There will be one overall repo in github for all of Ansible.
    - We will remove the /{core,extra}/ par of the directory structure
    - `lib/ansible/modules/core/commands/shell.py` -> `lib/ansible/modules/commands/shell.py`
  - We will start tagging modules to imply a "state" that the modules is in.
  - States cover both how usable the module is and how the module is supported.
    Full proposal with list of states and format of metadata here:
    - https://github.com/ansible/proposals/issues/30
  - Github Issues and PRs
    - All issues will be associated with one repo, since there will be only one repo!
    - Move and close pr’s and issues so they end up in the right place. 
    - *Existing PRs that are in flight will have to be reviewed and moved.*
  - Packaging
    - Sometime towards 2.3 (early to mid 2017) we will split packaging along functionality lines, *not* along support lines.  The reason for this one is that it decreases the likelihood that that modules would move among packages, which would be a major headache for users.
     - Rely on tagging in metadata to describe support levels rather than either packaging or repositories.

## Before Merge
  - Ensure the sub directories under `ansible-modules*/` are alined, some modules may need `git mv` in advance to ensure categories are the same
  - <strike>Define metadata format -- Sooner is better as many things depend on this</strike>
     - Update `validate-modules` to enforce this, though should be configured to only WARN initially apart from on new modules
  - <strike>Define tags we’re using for metadata(Toshio)</strike>
  - Code for PluginLoader to extract metadata and associate it with modules in a way that other code can emit warnings based on the metadata.
  - packaging -- setup.py needs to generate separate manifests via manifests
    - `--version` updated
  - ansible doc generation will need to change.
    - Replace repo label with metadata about supported by Ansible vs supported by Community
    - Update "To submit an update to module docs, edit the 'DOCUMENTATION' metadata in the core and extras modules source repositories."
  - Rpm/Deb and pip packaging concerns… think we can see how to address things for rpm and deb but pip will need thought.  Somewhat mitigated by splitting packages via function rather than support but still some hard problems here.
  - Scripts needed to migrate PRs.  Maybe migrate issues as well?
  - Script or procedure to combine the repos.
  - Script or update to the release playbook to generate multiple tarballs
  - Update to code that drives testing to handle single repo (targeted PR testing may be harder.  Other things are just different)
  - Bot code needs to be updated to handle metadata - policy differences for ansible supported vs community supported and handling of metadata at all.
  - Documentation
    - How to develop
      - ./README.md
      - ./lib/ansible/modules/core/CONTRIBUTING.md
      - ./lib/ansible/modules/extras/GUIDELINES.md
      - ./lib/ansible/modules/extras/CONTRIBUTING.md
      - ./docsite/_themes/srtd/footer.html
      - ./docsite/rst/developing_modules.rst
      - ./docsite/rst/modules_extra.rst
      - ./docsite/rst/community.rst
      - ./docsite/rst/dev_guide/developing_modules.rst
      - ./docsite/rst/developing_modules.rst
      - ./docsite/rst/modules_extra.rst
      - ./docsite/rst/community.rst
      - ./docsite/rst/dev_guide/developing_modules.rst
      - ./docsite/rst/modules_core.rst
      - ./docsite/rst/common_return_values.rst
  - Functional
    - ./lib/ansible/modules/core/source_control/git.py
    - ./lib/ansible/modules/core/system/mount.py
    - ./lib/ansible/modules/core/system/cron.py
    - ./lib/ansible/modules/core/packaging/os/apt.py
    - ./lib/ansible/modules/extras/cloud/amazon/ec2_customer_gateway.py
    - ./lib/ansible/module_utils/ismount.py
    - ./lib/ansible/plugins/lookup/password.py
    - ./ticket_stubs/_module_issue_move.md
    - ./ticket_stubs/_module_pr_move.md
    - ./ticket_stubs/module_repo.md
    - ./hacking/unify_repos.sh
    - ./hacking/module_formatter.py

## During Merge

  - Lockdown
    - Remove git commit rights to ansible-modules-core and ansible-modules-extras
  - Migrate
      - Migrate (with history) ansible-modules-core and ansible-modules-extras
        - https://github.com/ansible/ansible/blob/devel/hacking/unify_repos.sh 
      - Migrate open issues
        - Use Bot to add automated link to migrate tool
        - Give users a message and link to web tool to migrate
        - After some time, migrate rest of open issues for the users via the web tool (loses ownership but we can CC the owner.  They’ll just lose the ability to close/open/etc themselves)
      - Migrate PRs
        - Do we need a script here
          - Similar to GitHubs’s manual steps for merging a PR
          - We’re not sure if we can do this… It’s not easy as the files could change location and commit hashes will be different.  Can we drive this via git format-patch?  Can we make a tool that migrates what it can and only then punt on the rest?
          - We could write a script that will do this but the user would have to run it as PRs need to come from a fork of ansible/ansible that the user controls
  - Restrict permission on ansible-modules-core ansible-modules-extras
    - Restrict writing to `devel` on the two module branches
    - Change issue templates & Bot to auto close Issues
  - Unlock any branches as needed

## After Merge

  - Re-enable commit rights (needed for new releases on the stable-2.x branches)
  - Allocate lots of time in schedule to deal with fallout
  - Revisit the discussion regarding splitting the distribution into multiple packages
    - Ability to release groups of modules on their own, e.g. (modules-extras) could be released more often that Ansible Core Program
    - It hasn't yet been decided if we will split into multiple packages

## Long time after Merge

  - Close all Issues & PRs in old repo (that were created a long time ago - Newer PRs may exist for bugfixes for 2.0 or 2.1) 
