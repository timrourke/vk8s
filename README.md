# vk8s

An Ansible + Vagrant provisioning script for installing Kubernetes on Ubuntu
xenial64 VMs.

Based heavily on [bsder's excellent tutorial originally published on
DigitalOcean's site.](https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-11-cluster-using-kubeadm-on-ubuntu-18-04)

## Note

I do not recommend using this for anything important! This provisioning script
is purely for my own desire to understand Kubernetes more deeply, not for real
services. That said, any feedback is more than welcome, as I am quite new to k8s.

## Getting started

- [Install Vagrant](https://www.vagrantup.com/downloads.html)
- [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Clone this repo
- cd into the root of this project
- Run the command `vagrant up`