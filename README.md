# clusterer
An two server deployment of MaaS HA

## Server Configuration

Vanilla Ubuntu 16.04 Server

ifdown eno1

auto eno1
iface eno1 inet static
	address 10.1.2.51
	netmask 255.255.255.0
	gateway 10.1.2.1
	dns-nameservers 192.168.0.1

ifup eno1

apt install openssh-server openssh-client

## Client Installation

user@master$ cd .../maasive

Create appropriate inventory file such as:

server01  ansible_host=10.1.2.51
server02  ansible_host=10.1.2.52

[servers]
server[01:02]

[servers:vars]
public_iface = enp0s3
manage_iface = enp0s9

[maas_servers:children]
servers

[maas_region:children]
maas_servers

[maas_rack:children]
maas_servers


user@master$ ssh-keygen -t rsa -f ansible/security/ansible_rsa  -q -N ""

update remove_user in ansible.cfg

root@master$ apt install software-properties-common
root@master$ apt-add-repository ppa:ansible/ansible
root@master$ apt update && apt upgrade
root@master$ apt install ansible

user@master$ ansible-playbook -i inventory/<inventory> -l maas_servers --ask-pass --ask-become-pass playbooks/init.yml

user@master$ ansible-playbook -i inventory/<inventory> playbooks/maas.yml

## Contributing

1. Fork the project repo
2. Create your feature branch: `git checkout -b myfeature`
3. Commit your changes: `git commit -am 'Added my feature'`
4. Push to the branch: `git push origin myfeature`
5. Submit a pull request

## Credits

Steve Marvell (SM) - Systems Architect - Unipart Digital

## History

TODO

