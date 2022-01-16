# Diff files as a 'directory compare'

When invoking a diff between two commits with changes in multiple files, git shows the diff one file at a time. Depending on how many files are in the diff, this can really slow down your workflow since the diff tool needs to open and close for each individual file. It would be nice to see which files changed in a 'directory compare' format, where each file can be selected and the diff viewed without exiting the tool.

This is already possible with the `--dir-diff` option. Use this with difftool like this:
> `git difftool --dir-diff`

Note that because this is implemented by comparing the files in two temporary directories, you cannot directly edit the files during the diff.

# References
- https://stackoverflow.com/questions/2113889/git-difftool-to-give-directory-compare
- https://git-scm.com/docs/git-difftool
