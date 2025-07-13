#setup password less authentication


## 1. Generate an SSH key on your local machine (if not already):

```
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "your_email@example.com"
```

## 2. Copy your public key to the EC2 instance using your existing .pem file:


```

ssh -i "/Users/cholaraju/Downloads/Ansible EC2 Key.pem" ubuntu@35.170.192.54 "mkdir -p ~/.ssh && chmod 700 ~/.ssh"
cat ~/.ssh/id_ed25519.pub | ssh -i "/Users/cholaraju/Downloads/Ansible EC2 Key.pem" ubuntu@35.170.192.54 "cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"

```

## 3. Ensure SSH config on your instance allows public key login
Check this file on the EC2 instance:

```
sudo nano /etc/ssh/sshd_config
```

And confirm:

```
PubkeyAuthentication yes
PasswordAuthentication no

```
Then restart SSH:

```
sudo systemctl restart ssh

```

## 4. Test SSH login:

```
ssh ubuntu@35.170.192.54

```

You should be logged in without a password or .pem file, as long as:

Your public key is in the EC2’s ~/.ssh/authorized_keys

Your private key is at ~/.ssh/id_ed25519

File permissions are correct

##Optional: Ensure permissions

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```
And on the EC2 instance:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

```

