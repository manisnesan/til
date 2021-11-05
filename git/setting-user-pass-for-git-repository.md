# Setting up username & password for git repository cloned using http

I normally clone the repository using ssh protocol and would add the public key as part of the repository to avoid repeating the username and password.
In a recent project I had to clone the repository using http. But it is annoying to repeat the username and password for every git operations. 

This [nice article](https://www.shellhacks.com/git-config-username-password-store-credentials/) showed how to add user and pass as part of your remote url and have it as part of the git config.


**Cloning the repository for the first time**
```bash
$ git clone https://<USERNAME>:<PASSWORD>@github.com/path/to/repo.git
```
This stores the username and password in `.git/config`.

**Setting the credentials for already cloned repository**

If we have cloned the repository, update the remote url by 
```bash
$ git remote set-url origin https://<USERNAME>:<PASSWORD>@github.com/path/to/repo.git
```

**Validate the url**

`$ git config --get remote.origin.url`
