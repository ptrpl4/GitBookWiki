# 🛶 SSH

#### links

- https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#ssh-overview

## .ssh folder

### Structure

```bash
tree ~/.ssh

.
├── one_more_key
├── one_more_key.pub
├── config
├── id_rsa
├── id_rsa.pub
└── known_hosts
```

## Setup Instructions

### Create Keys

```bash
cd ~/.ssh

# create key with rsa type, 4096 bits, with email in comment
ssh-keygen -t rsa -b 4096 -C "your_dev_email@example.com" -f ~/.ssh/dev_key

ssh-keygen -t rsa -b 4096 -C "your_corporate_email@example.com" -f ~/.ssh/work_key

# edit config file
nano ~/.ssh/config
```

example config (autoload and multiuser)

```bash
cat ~/.ssh/config

Host github
  HostName github.com
  User ptrpl4
  AddKeysToAgent yes
  IdentityFile ~/.ssh/dev_key
  UseKeychain yes
  IdentitiesOnly yes
  
Host bitbucket-corporate
  HostName bitbucket.org
  User best_dev
  AddKeysToAgent yes
  IdentityFile ~/.ssh/work_key
  UseKeychain yes
  IdentitiesOnly yes
  
Host pi-local
  HostName 192.168.1.138
  User remote
  IdentityFile ~/.ssh/dev_key
```

for one user

```sh
cat ~/.ssh/config

Host gitlab
  HostName github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/dev_key
```

### Add to ssh-agent

```bash
# manual save passphrases to keychain
ssh-add --apple-use-keychain ~/.ssh/dev_key
ssh-add --apple-use-keychain ~/.ssh/work_key

# load all from keychain
ssh-add --apple-load-keychain

# check added
ssh-add -l

# del active keys
ssh-add -D


## Others
# change passphrase (if needed)
ssh-keygen -p -f ~/.ssh/id_rsa

# start an SSH Agent for the current shell:
eval $(ssh-agent)

# kill the currently running agent:
ssh-agent -k
```

## Copy Public Key

```bash
cat ~/.ssh/id_rsa.pub | pbcopy
```

## SSH Connect

```bash
ssh username@remote_host

# ssh with identity_file
ssh -i ~/.ssh/corporate_key root@192.168.1.1

# use default key
ssh root@123.32.11.10

# without checking host
ssh -o StrictHostKeyChecking=no username@remote_host 
```

## Server-side

Add pub-key to the server

```bash
# will create a .ssh directory and authorized_keys file if they didn't exist
ssh-copy-id -i ~/.ssh/id_rsa.pub my-host-server
```

## Commands

```bash
scp my-host-server:~/Documents/manifest ~/Project/
```

After `scp`, we type the name of the server from the config file (or [username@ip-address](mailto:username@ip-address) of the server), the `:` sign, the path to the file on the server space, and after that the path on the local machine.
