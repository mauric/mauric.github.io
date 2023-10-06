---
layout: post
title:  "Connect a local repository to a remote using SSH keys authorization"
date:   2023-09-07 17:49:00 -0300
categories: jekyll weekly
permalink: /:categories/:year/:month/:day
---

# Create a repository fom scratch using the terminal connect it to a remote repository and start working

[Paragraph 1: review as a whole]: #
**I would like to start by explaining the basic** steps to create a repository using only a Linux/Mac terminal. This is a very common scenario when
you start a project from scratch or decide after a few hours of work that it's time to track the code and be able to work collaboratively 
in the future when you upload it to Github, Gitlab or other platforms. 

[Paragraph 2: reviewed, check only at the end as a whole]: #
**The idea is to use ssh keys authorization** instead of credentials to gain access to the repository. This is also useful if you are developing in embedded systems, IoT devices or servers that you can only access through a secure shell connection. It provides more security and control about 
which devices can actually connect to your repository and helps to avoid the use your credentials everywhere.

[Paragraph 3: not review with tool]: #
**These instructions assume that you have access to your Github account and can create a new empty repository**
create one that will be linked to your local repository. There is a way to avoid accessing the website by using the recently released Github CLI. 
This tool can create a new repo from the terminal as well as initialize the local one. 
In all of these cases you can simply initialize a Git repository in your local working directory and attach it to the previously created remote repository in Github.

In order to do it the first step is check if you have already created you private and public keys. If you don't have 
them follow this steps 

**Check if you already have ssh keys** looking into the `~/.ssh/` directory for a pair of files named like `id_ed25519` and `id_ed25519.pub` (this are the default names)

If you have a pair of keys to use you can skip this step. If is not the case or you simply want to have
a new set of keys this an specific connection let's use `ssh-keygen` to create a new pair of keys 


```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
**The [EdDSA](https://ed25519.cr.yp.to/): Edwards-curve Digital Signature Algorithm** is used here because is the new standard 
recommended by Github in the [SSH keys generation documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). If your system doesn't support Ed25519 algorithm check the documentation
there are some alternatives there. The command will ask if you want to set a passphrase but you leave it empty and press enter.
Passhphrase have some benefits, they serve to encrypt the key locally providing more security. If someone gain access to your computer will
not be able to read or use the private key, because it's encrypted using the passphrase. The downside is that you need to enter it every time 
you create an SSH connection. To avoid enter it every time you connect you can temporarily cache your passphrase using ssh-agent. 

I leave the passphrase empty for this example so, the output in the terminal something like this:

```bash
Your public key has been saved in /Users/USER/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxx your_email@example.com
The key's randomart image is:
+--[ED25519 256]--+
|                 |
| this info is    | 
|     public      |
|                 |
+----[SHA256]-----+
```

**The [fingerprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) is based on** the host's public key and is used to make the identification or verification of the host you are connecting to. 
Of course the fingerprint of the machine you are trying to connect could change if the public key has been changed but, a priori this will help
you to identify if you are not being a "man-in-the-middle" attack where somehow your ssh connection is being rerouting to a different, unknown and malicious host. 
This ASCII "random art" is as public as your public key and is used to easy recognize the 

**The randomart is meant to be** an easier way for humans to validate keys. Is known that humans are quite slow an innacurate at doing comparison of complex meaningless string like
an hexadecimal representation of the key fingerprint. To solve this problem the random art system 
make use of computer generated art using the ["Hash Vizualization"](https://users.ece.cmu.edu/~adrian/projects/validation/validation.pdf)technique to replace meaningless string with structured images. 


### Initialize the ssh-agent

Is necesary to validate that ssh-agent is enable for you system. In most linux systems the agent 
is started and configured at login automatically. This agent will use your private keys to manage all 
your connections. 
I just checked if the agent is already running in my system by checking if the next two variables are defined `$SSH_AGENT_PID` for linux and 
`$SSH_AUTH_SOCK` for mac. 

The man pages of ssh-agent explain two main ways to get an agent set up. The first is at the start of an X session and the second is to use a login session.
We use the second method. When a ssh-agent is started it print the needed shell command to set its enviroment variables. Which turns into evaluate it in the callling shell for example:
```bash
$ eval "$(ssh-agent -s)"
> Agent pid 37163
```
This command can possibly need to use `sudo` permissions to start the agent but it depends on the environment. You 
can check the ssh-agent manpages in order to learn about a more deep and detail setup and configuration of the agent. 

### SSH additional configurations 

The configuration are stored in a file located in ~.ssh/config if this file doesn't exist you can just create it and add
the following lines:

```bash
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

```
Then add the shh private key at the ssh-agent and store the passphrase in the keychain to avoid enter each time you 
create a connection. In this example the name of the private key is "id_ed25519" replace it by your corresponding key name. 

```bash
$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
> Enter passphrase for /Users/you/.ssh/id_ed25519: [type here your passphrase]
> Identity added: /Users/you/.ssh/id_ed25519 (your_email@example.com)

```
## Github account configurations

Let's easily copy your public key with the next command:

```bash
$ pbcopy < ~/.ssh/id_ed25519**.pub**
```

Go to your Github account setting in the "SSH and PGP keys" section and create a new key, set a name and paste
the your previous copied public key into the section. This will allow the connection of your computer with Github repositories. 

## Testing the successfull connection

In order to test if the connection succeed we can proceed 
trying to clone a repository from your account. Let's use new repository we have create for this project. 

```bash
$ % git clone git@github.com:THE-NAME-OF-THE-REPO
```
```
Cloning into 'name-of-the-repo-folder'...
The authenticity of host 'github.com (--IP ADDRESS HERE --)' can't be established.
ED25519 key fingerprint is SHA256:--FINGERPRINT HERE--.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
remote: Enumerating objects: 1614, done.
remote: Total 1614 (delta 0), reused 0 (delta 0), pack-reused 1614
Receiving objects: 100% (1614/1614), 8.07 MiB | 4.78 MiB/s, done.
Resolving deltas: 100% (826/826), done.
```
After accept the connection the repo is downloaded an the Github host is stored as a known host hence the connection has been successfull. 

## Import source code using the command line

Following the [Github documentation](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github?platform=mac)

1. Open Terminal
2. Navigate to the root directory of your project
3. Initialize the local directory as a Git repository:
    1. `git init -b main`
4. Add the file sin your new local repository :
    1. `git add . `→ this command will stage all the files in the local repo
    2. `git reset HEAD YOUR-FILE` → will remove from stage any specific file
5. Commit your changes
    1. `git commit -m "first commit"`
    2. just in case you need to remove commit → `git reset —soft HEAD~1`
    3. commit the file again

Now the repository is initialized and a new hidden folder called `.git` has been create in your directory 
to store all the versions information like commits.

## Adding a local repository to GitHub using Git

Look for the SSH URL of your previously created empty repository (is the same you use to clone it). In order to avoid errors do not create any additional file like README, license or gitignore files.
Copy the remote repository URL.

1. Open Terminal
2. Navigate to your local project
3. Link you local repository with the remote repository where the will be pushed using the SSH URL
    1. git remote add origin <REMOTE_URL>
    2. git remote -v → to verify

Now the local repo is "attached" to the repo created in Github which enable us to push changes into it. 
Push the changes doing:

```bash
git push -u origin main
```
Check if your source code is available in the repository website.

### Troubleshooting 

One issue that appeared when following this procedure was related to this message 
```
fatal: refusing to merge unrelated histories
```
Basically what is happening is that I have accidentally initialized the repo on the Github side with a 
license file which is associated to an initial commit. 
This was creating a conflict with my already initialized local repository that also have an initial commit. I got errors While
trying to pull or push changes. This situation is preventing me to continue with a normal git workflow since git has no idea how the two projects are related.  

A solution that works for me is in this [article](https://www.educative.io/answers/the-fatal-refusing-to-merge-unrelated-histories-git-error) associate with a more detailed explanation about the issue. 

## References

- [Generating a new ssh key and adding it to ssh agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [Github doc: connection to Github with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh)
- [Github tutorial](https://kbroman.org/github_tutorial/)
- [Azure ssh documentation](https://learn.microsoft.com/en-us/azure/devops/repos/git/gcm-ssh-passphrase?view=azure-devops)
- [Standford Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/pr01.html)


