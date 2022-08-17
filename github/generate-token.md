# Generate personal access token

GitHub wants you to use tokens to access private repositories (and other access-restricted stuff). The way to generate them is not obvious at first glance.

1. User icon in upper right -> Settings
2. Developer settings, all the way at the bottom
3. Personal access tokens, at the bottom again

If you just want to read/write code then `repo` scope should be sufficient.

To use the generated token, see [Use token instead of password](/git/token-instead-password.md).

# References
- https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
