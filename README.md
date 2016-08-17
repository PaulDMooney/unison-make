# Compile and install Unison
Currently this project is a quick and dirty project based on my Ansible-Control-Machine project, which leverages ansible to checkout and compile Unison.
This project will vagrant up an ansible control machine as well as a target server to build unison on. The unison.yml playbook included does the compilation work.

What is the point of this?
Unison file sync packages that are available for ubuntu do not come with the unison-fsmonitor which is needed to use Unison's `repeat=watch` feature.
To get the unison-fsmonitor, unison has to be compiled from source, which includes a few other dependencies and a couple of gotchas. This project provides a playbook (which should become a role), that can do the work of checking the project out and building it.

##TODO:

1. Update this documentation to become more relevant for this project.
1. Turn unison playbook into an ansible-galaxy role.
1. Lots of cleanup and ensuring unison playbook is idempotent.

## Prerequisites
1. Virtualbox
1. Vagrant (latest. 1.8.4 at the time of writing)

## Getting Started
1. Run `vagrant up`
1. Run `vagrant ssh control`
1. Optional: Run `ansible-playbook -i hosts/local control.yml -e load_secrets=true --ask-vault-pass` and the password is `password`
1. Run `ansible-playbook -i hosts/local unison.yml`

## Testing
You will need another machine (could be your Host OS) with unison installed. Place the test.prf in the $USER_HOME/.unison directory and run `unison test`.

### Windows
Unison can be installed from Chocolately for Windows: https://chocolatey.org/packages/unison
You will also need an ssh implementation on your PATH. I suggest installing cygwin or babun and putting their binaries on your PATH.
* Note * That the unison-fsmonitor for tracking changes to files on Windows does not work in a cygwin/babun shell. You must use command prompt (or maybe powershell)
