rc-town instance
===

This is a playground for rc-town.

Access the vm with `vagrant ssh` from this directory, and inside the vm, this directory is mounted at `/vagrant`.

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
