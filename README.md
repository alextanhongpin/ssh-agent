# ssh-agent

## Starting

```bash
# Start
$ eval `ssh-agent`

# Verify
$ echo $SSH_AGENT_SOCK
```

## List private keys accessible to the agent

```bash
$ ssh-add -l
```


## Create Github SSH Key

Create a new SSH key at the default location:
```bash
$ ssh-keygen -t rsa -b 4096 -C "your_github_email@mail.com"

Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

For macOS Sierra, you need to modify `~/.ssh/config` to automatically load keys into ssh-agent and store passphrases in your keychain:

```bash
Host *
  AddKyesToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

Add the SSH private key to the ssh-agent and store your passphrase in the keychain. 

```bash
$ ssh-add -K ~/.ssh/id_rsa
```

Add to Github:

```bash
# Copies the contents of the id_rsa.pub file to your clipboard.
$ pbcopy < ~/.ssh/id_rsa.pub

# Next, go to the Github Page > Settings > SSH and GPG Keys > New SSH Key > Add SSH key.
```

## Add SSH in AWS


```bash
# ssh into the EC2 instance.
$ ssh -i palnetwork-bootnode.pem ubuntu@ec2-xx-xxx-x-xx.ap-southeast-1.compute.amazonaws.com

# Create a new user.
$ sudo adduser johndoe

# Switch shell session to the new account.
$ sudo su johndoe

# Create a .ssh directory, and change the permission to 700 (only the file reader can read, write and execute the directory).
$ mkdir .ssh
$ chmod 700 .ssh

# Create an empty file called authorized_keys in the .ssh directory and change its permission to 600 (only the file owner can read or write the file)
$ touch authorized_keys
$ chmod 600 authorized_keys
$ curl -O https://github.com/<username>.keys > ~/.ssh/authorized_keys
 
# Alternative
$ cat ~/.ssh/id_rsa.pub | ssh -i your.pem ubuntu@ec2-xx-xxx-x-xx.ap-southeast-1.compute.amazonaws.com "cat >> ~/.ssh/authorized_keys"
```

## Reference
- https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
- https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/
