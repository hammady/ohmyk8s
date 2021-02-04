# Oh My Kubernetes
An Ansible Playbook to automate the configuration of (Ubuntu Linux) machines
to be ready for Kubernetes development. It cuts down the development
environment preparation from tedious hours to less than 5 minutes.

## Included software
- [Git](https://git-scm.com/)
- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/)
- [Kubernetes (microk8s)](https://microk8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [helm](https://helm.sh/)
- [skaffold](https://skaffold.dev/)
- [k9s](https://github.com/derailed/k9s)
- [SOPS](https://github.com/mozilla/sops) and [helm secrets plugin](https://github.com/zendesk/helm-secrets)
- [azure-cli](https://docs.microsoft.com/en-us/cli/azure/)
- [ingress-nginx](https://kubernetes.github.io/ingress-nginx/)
- [kube-ps1](https://github.com/jonmosco/kube-ps1)
- [tmux](https://tmuxcheatsheet.com/) and [.tmux](https://github.com/gpakosz/.tmux)
- [kubectx/kubens](https://github.com/ahmetb/kubectx)

All of the above come with bash completion, whenever applicable.

## System requirements
1. [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW) on your controller machine (e.g. laptop) which is typically different from, but could be the same as, the target machine you are preparing. You also need a couple of Ansible collections, install using the command:
`ansible-galaxy collection install community.general community.kubernetes`
1. One or more SSH connections to the target machine(s) with public key authentication configured. You can do that by editing your `.ssh/config`. For example:
    ```bash
    # ~/.ssh/config
    Host awsdev1
        HostName 100.100.100.100
        User ubuntu
        IdentityFile ~/.ssh/id_rsa
        IdentitiesOnly yes
    ```

1. A configured Ansible hosts file with a section called `k8sdev`
containing all the machines you want to configure (yes you can configure many machines at once!). For example:
    ```ini
    # /etc/ansible/hosts
    [k8sdev]
    awsdev1 ansible_ssh_user=ubuntu
    1.2.3.4
    5.6.7.8
    ...
    ```

## Installation
To launch the installation process that will configure all target
machines at once, run the following:
```bash
ansible-playbook ohmyk8s.yaml
```
Sit and watch for the results, you should get a report similar to the below if everything goes well:
```
...
PLAY RECAP *******************************
awsdev1                     : ok=56   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
## Support
If there are any errors from the above command, please create an issue in this repo or drop
me a line at: github at hammady dot net.

If you have any suggestions or enhancements, please feel free to create a PR or discuss
with me. Please note that I am a total noob in Ansible, I learned it in the same day
I created this repo.