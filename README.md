# Oh My Kubernetes!
An Ansible Playbook to automate the configuration of (Ubuntu Linux x86_64 and ARM64)
machines to be ready for Kubernetes development. It cuts down the development
environment preparation from tedious hours to less than 5 minutes.

## Included software

The below are installed on all groups (`k8sdev`, `k8sdevlite` and `k8sdevdocker`):

- [git](https://git-scm.com/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [helm](https://helm.sh/)
- [skaffold](https://skaffold.dev/)
- [k9s](https://github.com/derailed/k9s)
- [sops](https://github.com/mozilla/sops) and [helm secrets plugin](https://github.com/zendesk/helm-secrets)
- [azure-cli](https://docs.microsoft.com/en-us/cli/azure/) and [kubelogin](https://github.com/Azure/kubelogin)
- [awscli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [kube-ps1](https://github.com/jonmosco/kube-ps1)
- [tmux](https://tmuxcheatsheet.com/) and [.tmux](https://github.com/gpakosz/.tmux)
- [kubectx/kubens](https://github.com/ahmetb/kubectx)
- [docker-cli](https://docs.docker.com/engine/reference/commandline/cli/)

All of the above come with bash completion, whenever applicable.

The below is installed on both `k8sdev` and `k8sdevdocker` groups:
- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/)

The below are installed only on the `k8sdev` group:
- [Kubernetes (microk8s)](https://microk8s.io/)
- [ingress-nginx](https://kubernetes.github.io/ingress-nginx/)

## System requirements
1. [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW) on your controller machine (e.g. laptop) which is typically different from, but could be the same as, the target machine you are preparing. You also need a couple of Ansible collections, install using the command:
`ansible-galaxy collection install community.general kubernetes.core` (Make sure that you are using Ansible > v2.9)
1. One or more SSH connections to the target machine(s) with public key authentication configured. You can do that by editing your `.ssh/config`. For example:
    ```bash
    # ~/.ssh/config
    Host awsdev1
        HostName 100.100.100.100
        User ubuntu
        IdentityFile ~/.ssh/id_rsa
        IdentitiesOnly yes
    ```

1. A configured Ansible hosts file with 2 sections called `k8sdev` and `k8sdevlite`
containing all the machines you want to configure (yes you can configure many machines at once!). For example:
    ```ini
    # /etc/ansible/hosts
    [k8sdev]
    awsdev1 ansible_user=ubuntu
    1.2.3.4 ansible_user=anotheruser
    5.6.7.8 ansible_user=ubuntu
    ...
    [k8sdevlite]
    ...
    [k8sdevdocker]
    ...
    ```

The above file can be stored in the default location `/etc/ansible/hosts` if the control machine is running Debian/Ubuntu or a similar distro. For other distros or other operating systems, you can store it in any location then supply `--inventory <file-location>` to the `ansible-playbook` command.

## Installation
To launch the installation process that will configure all target
machines at once, run the following:
```bash
git clone https://github.com/hammady/ohmyk8s.git
cd ohmyk8s
ansible-playbook ohmyk8s.yaml -Kv
```
Note: 
- `ansible-playbook` accepts -v for verbose mode (-vvv for more, -vvvv to enable connection debugging)
- `-K` to collect `BECOME password`

Sit (or stand!) and watch the results, you should get a report similar to the below if everything goes well:
```
...
PLAY RECAP *******************************
awsdev1                     : ok=56   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

## Caveats
- This only supports Ubuntu running on either x86_64 or ARM64 architectures.
Support for other distributions and architectures is possible but needs some work.

## Support
If there are any errors from the above command, please create an issue in this repo or drop
me a line at: github at hammady dot net.

If you have any suggestions or enhancements, please feel free to create a PR or discuss
with me. Please note that I am a total noob in Ansible, I learned it in the same day
I created this repo.
