# Proposal: Top-level module directory reorg

*Author*: Matt Davis <@nitzmahone>

*Date*: 2016/12/19

- Status: New
- Proposal type: process
- Targeted Release: 2.3
- Estimated time to implement: 2 days


## Motivation
It's very difficult to locate the code for a module today. The current file layout based on categories is somewhat arbitrary for a number of modules. Now that we have module metadata, the categories should be reflected there instead of via the physical file path in the repo. The modules should be filed simply by the first letter of their names, making it trivial to locate the code without a search, and reducing the need to move files around as metadata changes.


### Problems
- Locating a module's code can be difficult without search (or a priori knowledge of what category it lives in).
- Moving module code around as metadata changes is dumb.

## Solution proposal
Capture current module category information in module metadata. Move all modules to a simple flat directory structure by the module name's first letter (eg, `win_updates` would live under `modules/w`). Update doc generation to fetch category info from metadata instead of file path.

The 26 dirs under `modules/` aren't strictly necessary either, but the GitHub web UI truncates directory listings after 1000 entries (and in some cases won't show them at all if too large), so having a way to keep the file counts low-ish is important. Since the current module location code already traverses a couple layers of subtrees, we could easily add the second letter of the module names as a subtree underneath later, should a given letter's module file count approach 1000.


## Dependencies (optional)
- HTML docgen update
- Translate path category info to module metadata

