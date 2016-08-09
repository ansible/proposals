# Proposal: Proposals - Change how we manage modules in Ansible (the product) and the Ansible Community

 - Date: 09 AUGUST, 2016

 - Status: Rewritten from June proposal

 - Proposal Type: Community Development Process.

 - Targeted Release: 2.2 or possibly 2.3

 - Estimated time to Implement: 3-4 months.

## Motivation and Problems
There are currently two module repositories: ansible-modules-core, and ansible-modules-extras.  Traditionally this has been recognized to mean that there is a standard for Core modules (since they are to be supported by the Ansible Core team and Red Hat) and Extras (modules that are part of Ansible, but more broadly contributed to and less supported).  In practice, Extras became roughly the same thing as Core since Extras are both distributed with Ansible and supported by the Ansible Core team.  

That condition had the unintended consequences of forcing the Core team to set the requirements for Extras modules to be roughly the same as Core.  The result was that lots of modules contributed by the Ansible community were either rejected or asked to reach a certain level of robustness or feature-completion for inclusion.  This gave the community little room to have "starter" functionality, or a place to contribute simpler or differentiated functionality that is not necessarily a part of the "Core" product, but is nonetheless a good place to get other functionality into ansible.

## Solution
Ansible Core team and Contributors (from the Contributor Summit at the 2016 Ansible Fest San Francisco) propose to create categories of ansible Modules where Modules are grouped by a rough category of like-functionality (Nagios Modules would be grouped together, as would AWS, VMWare, etc).  There are a large number of implications for that working in the Ansible workflow and Community:
  - There *will not* be multiple repos.  There will either be one overall repo in github for Ansible and Modules.
  - We will start tagging modules to imply a "state" that the modules is in.
  - Current proposed states are:
    - Stable: We have confidence that these are stable for a broad spectrum of uses.
    - Curated: 
      - It has been reviewed by an Ansible Core team member.
      - Ansible is going to fix bugs in it if no one else is available.  Should mean that the module is stable in addition to us being on the hook to fix it.
    - Supported (SLA)
      - These are the core set of modules that Red Hat and the Ansible Core team support. 
      - We will fix these
      - We will set an SLA on problems with these modulse based on there criticality (to be defined elsewhere).
  - Issues
    - All issues will be associated with one repo, since there will be only one repo!
    - Auto-close pr’s and issues so they end up in the right place. 
    - *Existing PRs that are in flight will have to be reviewed and moved.*  
  - Packaging
    - Sometime towards 2.3 (early to mid 2017) we will split packaging along functionality lines, *not* along support lines.  The reason for this one is that it decreases the likelihood that that modules would move among packages, which would be a major headache for users.
  - TODOs during the next couple of months
    - Define metadata format -- Sooner is better as many things depend on this
    - Define tags we’re using for metadata(Toshio)
    - Code for PluginLoader to extract metadata and associate it with modules in a way that other code can emit warnings based on the metadata.
    - packaging -- setup.py needs to generate separate manifests via manifests
    - ansible doc generation will need to change.  Replace repo label with metadata about supported by Ansible vs supported by Community
    - Rpm/Deb and pip packaging concerns… think we can see how to address things for rpm deb but pip will need thought.  Somewhat mitigated by splitting packages via function rather than support but still some hard problems here.
    - Scripts needed to migrate PRs.  Maybe migrate issues as well?
    - Script or procedure to combine the repos.
    - Script or update to the release playbook to generate multiple tarballs
    - Update to code that drives testing to handle single repo (targeted PR testing may be harder.  Other things are just different)
    - Bot code needs to be updated to handle metadata - policy differences for ansible supported vs community supported and handling of metadata at all.
  - Documentation
    - Deliverables: Decision on how to write documentation for module_utils, plugins, and public API.
      - Toshio thinks he has a good handle on how we can do these but needs to figure out where to put them in the documentation tree. Item for him and dharmabumstead/docschick to work out.
    - Document module_utils API
    - Non-module plugins: lookup/filter/test/etc, with auto-doc. Look at plugins/connection/__init__.py for how this would look in the code.  Would need to pull this out into html docs
    - Multiple version documentation (having devel/last stable at least in docs).
