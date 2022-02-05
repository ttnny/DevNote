# Multiple SSH Keys on Single Machine



Config multiple SSH keys, e.g. GitHub and GitLab, on a single machine.

1. Setup new SSH keys as instructed:
   - [GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
   - [GitLab](https://docs.gitlab.com/ee/ssh/)

2. Modify `~/.ssh/config` file:

   ```
   Host github.com
     AddKeysToAgent yes
     HostName github.com
     IdentityFile ~/.ssh/github
   
   Host gitlab.com
     AddKeysToAgent yes
     HostName gitlab.com
     IdentityFile ~/.ssh/gitlab
     
   # Host company.github.com
     # AddKeysToAgent yes
     # UseKeychain yes
     # PreferredAuthentications publickey
     # HostName github.com
     # IdentityFile ~/.ssh/github_company_key
   ```

   *Note: If chose not to add a passphrase to the key, then omit the `UseKeychain` line.*