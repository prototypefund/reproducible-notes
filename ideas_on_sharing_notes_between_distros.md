# Sharing notes between distributions

*This is from the Reproducible Summit in Athens.*


* use cases for sharing notes

  + for upstream toolchain developers: what am i breaking
  + non toolchain upstream about what's wrong with their package
  + distro people wanting to check out the status and *history* on other distros and comparing

* we should keep it streamlined, like continue keep it in git.
* yaml seems to be cool for everybody


* issues
  + they can be fixed in different ways
  + they can be distro-specific
  + they can be implementation-specific
  + ........
* would be handy to have meta-issues

* history:
  + link to example patches and keep old issues

* linking packages between distros:
  + CPE (from CVE stuff)

* maintaining the notes:
  + we need a format where we can separate "upstream" issues, distro-specific issues, distro-specific versions, etc
  + different distros might have different way to fix issues

* moving stuff from wiki.debian.org/ReproducibleBuilds/ to the notes (→ lamby)

* format:

```
issue:
  url:
  description:
  distro:
    debian:
      state: fixed
      comment:

package:
  distros:
    debian:
      issues:
        - issue1
        - issue2 / free text            #   ← fixed issue
      comments:
      bugs: []
      version:
    freebsd:
      version:
      bugs: []
  comments:
  bugs: []
  issues: []
```

Before the reproducible builds summit in Paris

## Migration plans

- Which upstream tools would require to be updated to the new schema?
- Should we mass-migrate to a temporary namespace? (e.g.,
  v2.{notes,packages}.yml or such) and flip the switch at a pre-defined date?
- Provide a mass migration tool (i.e., take the version 1 and update the schema
  with same defaults, are all of the notes debian here?)


## Review of the current solution 

Comparing against the requirements listed above:

+ we need a format where we can separate "upstream" issues, distro-specific
  issues, distro-specific versions, etc:
  - A keyword "upstream" inside of the distro list may be useful to track
    issues upstream

+ different distros might have different way to fix issues:
  - Likewise, should we add a way to flag as duplicate between distros?


## Elaborating on the fields proposed

an inline copy of the current schema with "comments" is embedded so as to
describe what exactly would each field entail.

```
issue:
  url:          # a url/id of this issue
  description:  # a human-readable description of such issue
  distro:       # a list of distro-specific objects, keyed by distro name (e.g., debian, arch, freebsd)
    debian:
      state: fixed # state: open/fixed/unknown
      comment:     #

package: foo
  distros:      # a list of distro-specific objects, keyed by distro name (e.g., debian, arch, freebsd)
    debian:
      package: debfoo  # optional
      issues:   # issue pointers that match the id under issues.yml
        - issue1
        - issue2 / free text            #   ← fixed issue
      comments: # human-readable string containing any information that's distro-specific
      bugs: []  # a list of urls to bugs/issues opened regarding this package in the distro-bugtracker
      version:  # version that's affected by this package
    freebsd:
      version:
      bugs: []
  comments:     # cross-distro comments regarding this package
  bugs: []      # upstream bugs opened
  issues: []    # reproducible-issue identifiers that are cross-distro
```

These are quite open, as none of these fields are formalized in a specification (yet!)

### Issues with the current schema

Here's a list of the current issues found with the current schema (

- the cross-distro bugs and issues keys seem to need re-thinking
- the version key seems to be a string, which raises doubts about how to
  separate many-to-one issues when a package across different versions
- We may want to formalize the ER diagram of all of these keys, as to avoid
  issues like the one above. A proposal was added as er.gv and it can be compiled as:
    `dot er.gv -Tsvg -o output.svg`
