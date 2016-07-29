# Compile and install Unison
Currently this project is a quick and dirty project based on my Ansible-Control-Machine project, which leverages ansible to checkout and compile Unison.
This project will vagrant up an ansible control machine as well as a target server to build unison on. The unison.yml playbook included will does the compilation work.

TODO:
1. Update this documentation to become more relevant for this project.
1. Turn unison playbook into an ansible-galaxy role.
1. Lots of cleanup and ensuring unison playbook is idempotent.

## Prerequisites
1. Virtualbox
1. Vagrant (latest. 1.8.4 at the time of writing)

## Getting Started
1. Run `vagrant up`
1. Run `vagrant ssh`
1. Start using ansible

## Suggestions for including in your Project.
Since the purpose is to use this project as a reference to include in your own, there are a few suggestions on how to put this in your project.

1. Create a 'devops' subfolder in your project and copy all of the files there.
1. Copy and paste the 'ansible-control-machine' definition from the Vagrantfile here into the Vagrantfile of your own project. Perhaps put `autostart: false` on that VM.
Copy over the other files to sit in the same folder as your Vagrantfile

## Secrets
An example private ssh key is encrypted using [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html) in `keys/secrets.yml` and should be safe to store in a [private] code repo (with good password strength).
The complimentary public key is in `keys/sample_private_key.pub`. The `keys/secrets_unencrypted.yml` is provided for convenience, but is bad practice to keep in your repository.
`vagrant ssh` into the control machine, run `ansible-playbook control.yml -e load_secrets=true --ask-vault-pass`. When prompted, the password is `password`. The decrypted private key will be written to `/home/vagrant/private_keys/sample_private_key` and can be referenced at that path in `ansible_ssh_private_key_file` parameters in inventory files.

** Note ** Generate your own Public/Private ssh key pair, don't use this example.

** Note 2** Having a single private ssh key that is shared between any user with the password to decrypt is convenient, but not ideal. You lose the traceability of who executed an Ansible Playbook against your infrastructure.
It also becomes tedious to try to revoke access for an individual since it means creating new public/private key pairs, updating all of your servers with them, as well as updating your `secrets.yml` file and handing out the new password for it.
The ideal solution would be to have some kind of user management for your target servers (using LDAP for example), and have individuals use their own personal public/private key pairs to execute Ansible playbooks.
