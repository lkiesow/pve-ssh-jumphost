# Proxmox Virtual Environment: SSH Jumphost for Internal Network

Ansible script to [deploy an SSH jumphost for a Proxmox internal network](https://weblog.lkiesow.de/20220223-proxmox-test-machine-self-servic/ssh-jumphost-to-internal-network.html).

## Notes

- I used a CentOS Stream 9 LXC container
    - Should work with most distributions
- Make sure you can SSH into the container

## Additional Work

This will *not* set up port forwarding on the Proxmox server itself.
You need to [add an iptables rule for that](https://weblog.lkiesow.de/20220223-proxmox-test-machine-self-servic/ssh-jumphost-to-internal-network.html#make-forwarding-persistent).
In `/etc/network/interfaces`, add something like:

```
iface vmbr1 inet static
   address 10.0.0.1/16
   …
   post-up   iptables -t nat -A PREROUTING -p tcp -i vmbr0 --dport 2222 -j DNAT --to-destination 10.0.0.3:22
   post-down iptables -t nat -D PREROUTING -p tcp -i vmbr0 --dport 2222 -j DNAT --to-destination 10.0.0.3:22

```
