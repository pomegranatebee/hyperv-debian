# Install

1. Download an amd64 image from https://www.debian.org/distrib/netinst. "Small CDs or USB sticks" works.
2. Use the creation wizard in Hyper-V. Default settings are fine. Currently using "Generation 1" virtual machine for best compatibility.
3. Create root password and store in 1Password.
4. Create default user "caleb".
5. Create default password and store in 1Password.
6. Finish Debian install.

# Add default user to sudoers file

    su root
    sudo adduser caleb sudo

Log out and back in for update to take effect.

# Start SSH server

Check that SSH is running:
    
    systemctl status ssh

If not:

    sudo systemctl start ssh
    sudo systemctl enable ssh

If SSH is not installed:

    sudo apt-get install openssh-server

# Configure SSH on VM

Open configuration file for SSH:

    sudo nano /etc/ssh/ssh_config

Find and uncomment this line:

    Password Authentication yes

Create and secure the SSH config file:

    mkdir -p ~/.ssh
    chmod 700 ~/.ssh
    touch ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys

After running the above terminal commands, add the public key from remote machine, then reload SSH:

    sudo systemctl reload ssh

# Test SSH from remote machine and remove password access

Find IP address for VM:

    ipaddress ip address | grep -i eth0

On remote machine, SSH into VM using default username and password:

    ssh caleb@<vm_ip_address>

Open the authorized_key file and paste in the SSH public key for the remote machine:

    nano ~/.ssh/authorized_keys

Reload SSH:

    sudo systemctl reload ssh

Terminate session, then retest SSH connection using public key.

    exit
    ssh caleb@<vm_ip_address>

Open configuration file for SSH:

    sudo nano /etc/ssh/ssh_config

Find and recomment this line:

    # Password Authentication yes