# Alias commands

Git allows you to configure aliases for commands, which allows you to type the alias as shorthand for something else. This is most useful for commands that are commonly used, have longer names, or that are typically used with some parameters (which makes it longer to type the full command).

Aliases are controlled via configuration. Here are the ones I've found most useful:

- > `git config --global alias.b branch`
- > `git config --global alias.co checkout`
- > `git config --global alias.d diff`
- > `git config --global alias.dt difftool`
- > `git config --global alias.dd 'difftool --dir-diff'`
- > `git config --global alias.g 'log --graph --oneline'`
- > `git config --global alias.ga 'log --graph --oneline --all'`
- > `git config --global alias.rb 'rebase'`
- > `git config --global alias.rbc 'rebase --continue'`
- > `git config --global alias.rbi 'rebase -i --autosquash --rebase-merges'`
- > `git config --global alias.s status`

Remember that aliases are effectively a direct text substitution, meaning that they can be combined with the typical command parameters, for example with the `co` alias you can replace `git checkout -b my-branch` with `git co -b my-branch`

Note that the above commands set configuration globally, which affects all repositories for the current user. If you want to set configuration only for the current repository, omit `--global`.

# References
- https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases
