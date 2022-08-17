# Error: pathspec 'x' did not match any files

You might encounter an error where it seems that git is trying to work with files that you told it to ignore via the `.gitignore` file. A common one for me seems to be when I try to restore the working copy:

> `$ git restore *`

> `error: pathspec 'target' did not match any file(s) known to git`

The reason this happens it that the wildcard character `*` is expanded by the terminal, not by git. So it passes a name to git that git knows it should ignore, hence why git errors out. The solution is to be more specific about what you want to do (e.g. `git restore README.md`). At least in the case of restore, it seems that using `.` instead of `*` also works for when you want to restore everything.

# References
- https://stackoverflow.com/questions/11456403/stop-shell-wildcard-character-expansion
