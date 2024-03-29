rc-town instance
===

This is a playground for rc-town.

Test out rc-town with `ssh localhost -p 22222 -l town`. If your nothing shows up, try setting `TERM=xterm-256color`.

If you're the creator of the vagrant vm, you can also use `vagrant ssh -- -l town` to ssh in, but this does not seem to work for other users.

Access a shell in the vm with `vagrant ssh` from this directory. Inside the vm, this directory is mounted at `/vagrant`.

Vagrantfile created with `vagrant init hashicorp/bionic64`.

### Changes to the vm

- added user `town` with no password.

```
useradd -m town
passwd -d town
chsh -s rc-town town
chown root:root /home/town
cp /vagrant/bin/rc-town /home/town/
```

- New stanza in /etc/ssh/sshd_config (also had to remove existing `UsePAM` line)

```
UsePAM no

Match User town
        PasswordAuthentication yes
        PermitEmptyPasswords yes
        ChrootDirectory /home/town
        ForceCommand rc-town
        DisableForwarding yes
        # MaxSessions 1
```
