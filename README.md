# SWARM-CENTOS

Lance 4 VMs centos (node-0, node-1, node-2 et node-3) préconfigurées avec Docker, permettant de faire les TPs de la formation officielle "CN110 - Docker Swarm Application Essentials"

## Prérequis

Fonctionne sur une distribution linux ubuntu 20.04

Nécessite idéalement un CPU avec 8 thread et 16Go de ram

* Virtualbox (version testée: 6.1.18r142142): https://www.numetopia.fr/installer-virtualbox-sur-ubuntu-ou-linux-mint/
* Vagrant (version testée: 2.2.14): https://linuxize.com/post/how-to-install-vagrant-on-ubuntu-20-04/
* Ansible (version testée: 2.9.6): https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu

## Démarrage

```
vagrant up
```

## Accès ssh aux VMs

Exemple avec node-0

```
vagrant ssh node-0
```

## Initialisation de Swarm

Lors de l'initialisation du 1er manager Swarm, il faut impérativement préciser l'ip de l'interface ```eth1``` (voir avec la commande ```ip a```) avec l'option ```--advertise-addr```

Exemple pour node-0:

```
[vagrant@node-0 ~]$ docker swarm init --advertise-addr 192.168.77.20
```

## Accès sur le host

Le port 8000 des VMs est redirigé sur le host:

* node-0: 8000 => 9000
* node-1: 8000 => 9001
* node-2: 8000 => 9002
* node-3: 8000 => 9003

Ce qui permet d'accéder au port 8000 de chaque VM via la machine host


Si vous avez besoin d'ouvrir d'autre port des VMs, modifiez le fichier ```Vagrantfile```:

```
      node.vm.network "forwarded_port", guest: <port VM>, host: "#{<port host>+node_id}"
```

Puis pour appliquer le changement

```
vagrant up --provision
```