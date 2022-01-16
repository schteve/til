# Associate text editor

You may not like the default text editor that git uses out of the box. This can be controlled via configuration. Here are some that I have used:

VS Code
> `git config --global core.editor "code --wait"`

Notepad++
> `git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`

Note that the above commands set configuration globally, which affects all repositories for the current user. If you want to set configuration only for the current repository, omit `--global`.

# References

- https://docs.github.com/en/get-started/getting-started-with-git/associating-text-editors-with-git
- https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration
