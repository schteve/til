# Rebase the first commit

If you try to rebase against the first commit in a repository, you will get an error, for example:

> `git rebase -i HEAD^`<br/>
> `fatal: invalid upstream 'HEAD^'`

The solution is to rebase against the repo root instead:

> `git rebase -i --root`

# References

- https://stackoverflow.com/questions/22992543/how-do-i-git-rebase-the-first-commit/23000315
