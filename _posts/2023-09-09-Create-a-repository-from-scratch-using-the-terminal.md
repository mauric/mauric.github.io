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

[Paragraph 3: not review with tool]
**These instructions assume that you have access to your Github account and can create a new repository**
create one that will be linked to your local repository. There is a way to avoid accessing the website by using the recently released Github CLI. 
This tool can create a new repo form the terminal as initialize the local one as well. 
Well, in all of these cases you can simply initialize a Git repository in your local working directory and attach it to the previously created remote 
repository in Github.

In order to do it the first step is check if you have already created you private and public keys. If you don't have them follow this steps 
to create them. 

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
**The algorithm used to create the encrypted keys**, as you can appretiate is indicated by `ed25519` argument indicates that
the [EdDSA](https://ed25519.cr.yp.to/): Edwards-curve Digital Signature Algorithm is used which is the new standard recommended by Github in
 the [SSH keys generation documentation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). If your system doesn't support Ed25519 algorithm check the documentation there are some 
alternatives there. 

The next step is set a passphrase you can even choose one or leave it empty. The passhphrase serve to encrypt the key locally and if someone
gain access to your system will not be able to read the private key, because it's encrypted

A secure passphrase helps keep your private key from being copied and used even if your computer is compromised.

The downside to passphrases is that you need to enter it every time you create a connection using SSH. 
You can temporarily cache your passphrase using ssh-agent so you don't have to enter it every time you connect.

After entried a passphrase or leave it empty the output in the console is 

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
This ASCII "random art" is as public as your public key and is used to easy recognize the 

The randomart is meant to be an easier way for humans to validate keys.

Validation is normally done by a comparison of meaningless strings (i.e. the hexadecimal representation of the key fingerprint), which humans are pretty slow and inaccurate at comparing. Randomart replaces this with structured images that are faster and easier to compare.

https://users.ece.cmu.edu/~adrian/projects/validation/validation.pdf


### Inicio del ssh-agent

En mac es necesario levantar el ssh-agent para poder establecer la conexión ssh parece. Al ejecutar el comando siguiente obtenemos el PID del proceso

Is necesary to validate that ssh-agent is enable for you system. In most linux systems the agent 
is started and configured at login automatically. This agent will use your private keys to manage all 
your connections. 
I just checked if the agent is already running in my system using `ps` command to search for 

The man pages of ssh-agent explain how start an agent

     There are two main ways to get an agent set up.  The first is at the start of an X session, where all other windows or programs are started as children of the ssh-agent
     program.  The agent starts a command under which its environment variables are exported, for example ssh-agent xterm &.  When the command terminates, so does the agent.

     The second method is used for a login session.  When ssh-agent is started, it prints the shell commands required to set its environment variables, which in turn can be
     evaluated in the calling shell, for example eval `ssh-agent -s`.


```go
$ eval "$(ssh-agent -s)"
> Agent pid 37163

```
This command can possibly need to use `sudo` permissions to start the agent but it depends on the environment. You 
can check the ssh-agent manpages in order to learn about a more deep and detail setup and configuration of the agent. 



### Configuraciones en ssh-config
### Ssh-config 
Se debe verificar que el archivo de configuración ~.ssh/config existe y sino crearlo para agregar el siguiente contenido

The configuration file located in ~.ssh/config if this file doesn't exist you can just create it and add
the following lines:


```bash
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

```

Then add the shh private ky at the ssh-agent and store the passphrase in the keychain. In this example the name 
of the private key is "id_ed25519" but it should be replaced by the corresponding case. 


```go
$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
> Enter passphrase for /Users/you/.ssh/id_ed25519: [type here your passphrase]
> Identity added: /Users/you/.ssh/id_ed25519 (your_email@example.com)

```

## Add your ssh key into your account

Let's easily copy your public key with the next command:

```bash
$ pbcopy < ~/.ssh/id_ed25519**.pub**
```

Go to your Github account setting in the "SSH and PGP keys" section and create a new key, set a name and paste
the your previous copied public key into the section. 

## Testing the successfull connection

In order to test if the connection can be made succesfully we can proceed 
trying the brand new repository we have create for this project. 

```go
$ % git clone git@github.com:THE-NAME-OF-THE-REPO
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

References

- https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github?platform=mac
1. Open Terminal
2. Navigate to the root directory of your project
3. Initialize the local directory as a Git repository:
    1. `git init -b main`
4. Add the file sin your new local repository :
    1. `git add . `→ this command will stage all the files in the local repo
    2. `git reset HEAD YOUR-FILE` → will remove from stage any specific file
5. Commit your changes
    1. `git commit -m "first commit"`
    2. to remove commit → `git reset —soft HEAD~1`
    3. commit the file again

## Adding a local repository to GitHub using Git

Create and empty repository to avoid errors do not create any additional file like README, license or gitignore files.
Copy the remote repository URL. If you have set your SSH keys for authorization then select the SSH URL.

1. Open Terminal
2. Navigate to your local project
3. Link you local repository with the remote repository where the will be pushed using the SSH URL
    1. git remote add origin <REMOTE_URL>
    2. git remote -v → to verify

Push the changes with

```go
git push -u origin main
```

## Troubleshooting unrelated histories

One issue that appeared when following this procedure was related to this message 
```
fatal: refusing to merge unrelated histories
```

Basically what is happening is that the repository I have created using the Github website has been initialized with a 
license file, the best option is to create an empty repository. Github associate that license file to an initial commit. 
Since I have create a new local repository using the terminal and add a initial commit this is entering in conflict 
with the license initial commit when trying to pull changes in order to push into the remote repo my local changes. 
And this situation is preventing me to continue with a normal git workflow since git has no idea how the two projects are related.  

There a solution in this [article](https://www.educative.io/answers/the-fatal-refusing-to-merge-unrelated-histories-git-error) associate with a more detailed explanation about the issue. 

## References

- [Generating a new ssh key and adding it to ssh agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- [Github doc: connection to Github with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh)
- [Github tutorial](https://kbroman.org/github_tutorial/)
- [Azure ssh documentation](https://learn.microsoft.com/en-us/azure/devops/repos/git/gcm-ssh-passphrase?view=azure-devops)
- [Standford Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/pr01.html)


