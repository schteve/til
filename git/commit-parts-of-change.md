# Commit parts of change

Sometimes you made multiple changes to a file but want to commit these changes separately. This can be accomplished in several ways. The simplest is to use the `-p` (patch) option when adding the file:

> `git add -p`

This brings up an interactive session where you can decide whether each "hunk" should be staged or left in the working directory.

A more featureful way to do this is using the `-i` option, which gives a more featureful interactive session where files may be patched, updated, reverted, and more.

> `git add -i`

# References
- https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging
