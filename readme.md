# Install and configure Wireguard VPN

Here is a basic set of an ansible playbooks designed for automatic install and configure Wireguard VPN on your server. 

Also playbooks allows you to automatic add new clients. All clients configurations are backuped in /etc/wireguard/clients.

NOTE: Playbooks are designed for ubuntu and checked on ubuntu 20.04.

# Step by step guide:

## Figure out connector name on server: 

> `ip route list default`

Output will be like 
> `default via 195.2.79.1 dev `***ens3***` proto static`


In these example connector name is ***ens3***. It is used in host_vars then.

## Prerequisites

1. Configure credentials to connect to your host in *inventory.yml*
2. Set host variables in host-vars/*host-name*.yml

## Availiable commands

Here is commands in order to run on empty server

By default they will be executed on *test* host (In my case it was docker container). If you want to run it on your host use additional parameter `--extra-vars "target_host=<host_name>"`, replace *<host_name>* to your host.

`ansible-playbook -i inventory.yml install-wireguard.yml`

`ansible-playbook -i inventory.yml configure-wireguard.yml`

`ansible-playbook -i inventory.yml create-client-qr.yml` - here don't forget to scan qr code

`ansible-playbook -i inventory.yml --tags start control-wireguard.yml`

## Adding new user

`ansible-playbook -i inventory.yml create-client-qr.yml`

`ansible-playbook -i inventory.yml control-wireguard.yml` - here control-wireguard.yml will restart wireguard. If wireguard is stoped, then just add parameter `--tags start`.
