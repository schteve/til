# Use token instead of password

If you generated an access token for your repositories hosted on GitHub the next step is to use the token. You can simply use it in place of your password which you may be prompted for the next time you access a repo that requires a token. However this might not happen if you already had a token or password saved.

The simplest way seems to be to try a few times and git should recognize that the password / token is bad and prompt for a new one. If that doesn't work you can try to reject the credentials using `echo "url=$(git remote get-url origin)" | git credential reject`.

# References
- https://stackoverflow.com/questions/60640707/how-can-i-reset-the-password-saved-by-git-from-a-linux-terminal
