## Identity Separation
Context seperation is the key to maintaing your privacy. This article shows some tricks and technical approchases how to do it in practice.

> **DISCLAIMER: If you want to do some "high profile" stuff, this is NOT ENOUGH!** In that case DO MORE RESEARCH. You need to do stuff like:  hardware separation, be always behaind Tor (with multiple kill switches), implement multiple checks in every process you have to not doxx yourself, **AND MORE...**

### Priciples

#### Default is bad
Having default settings usually means fallback to some _default_ identitiy. This is easy way to doxx yourself.

### Servicies

#### SSH (not just) for Git
1. `vim ~/.ssh/config`
2. Create file with content like this:
   ```bash
   Host gitlab.company-x.com
     HostName gitlab.company-x.com
     User git
     IdentityFile ~/.ssh/id_rsa_company-x

   # All other domains will fallback to this and will be redirected 
   # to localhost to avoid accidentally doxxing yourself by ssh login    
   # attempt with different keys
   Host *
     HostName localhost
   ```
3. To create aliases for different (for example Github) repositories add
   ```bash
   Host johndoe.github.com  # this is aliad
     HostName github.com   # this is real hostname
     User git
     IdentityFile ~/.ssh/id_rsa_github_johndoe
   ```
   Think of it as a kind of DNS for Git 😄

**Extra step**: You can use KeePassXC to act as `ssh-agent` so it will unlock your SSH keys only when your KeyPass database is unlocked. See [this](https://keepassxc.org/docs/#faq-ssh-agent-how) and [this](https://www.techrepublic.com/article/how-to-integrate-ssh-key-authentication-into-keepassxc/) for more info.

#### Git
1. `vim ~/.gitconfig`
2. **Make sure there is no `[user]` section (delete it)**, remmeber **default is bad**!
3. Add 
   ```
   [includeIf "gitdir:~/workspace/<context-x>/"]
       path = .gitconfig-context-x
   ```
4. `vim ~/.gitconfig-context-x`
5. Add 
   ```
   [user]
       name = Identity X
       email = xxx@xxx-com
   ```
6. Use that folder only for stuff withing context of the given identity.
  
#### Browser profiles
Use profiles in the browser to separate identites. For example: if you work for different clients / you cntribute to different projects (you should do it under different names), you should create separate profile to prevent accidentally mixing your identities.

- [Firefox profiles](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) to isolate identities
- [Firefox extension: Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) to isolate between domains

## Sources
- [Simon's Blog | Git Identities and Ssh](https://simonbasle.github.io/2017/10/git-identities-and-ssh/)
