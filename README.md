# Ansible Kali Linux Build

## tl;dr

My Ansible playbook for automating Kali Linux setup with:
- Essential `APT` tools, GitHub `repos`...
- Customize `Terminal`, `Tmux` (also default shell is `zsh`)
- Configure Firewall `UFW` 
- Configure Logging `rsyslog`, `auditd`, `laurel`
- set`NOPASSWD` for current user in `sudoers`
- Install `vscode` + Extensions: `Python`, `PHP`, `Github Copilot`, `Snyk`

Based of: https://github.com/IppSec/parrot-build

---

## Installation

- Install ansible with apt
```shell
sudo apt install ansible
```

- Download Repo
```shell
git clone https://github.com/ndanilo8/ansible-kali-build
cd ./ansible-kali-build
```

- Install the dependency
```shell
ansible-galaxy install gantsign.visual-studio-code
```

- Finally, run ansible-playbook with your ansible vault
```shell
# get sudo token (valid for 15mins)
sudo whoami
# execute ansible-playbook
ansible-playbook main.yml --ask-vault-pass
```
_might take some time, let it run_

---

## `Audit` Best Practice Configuration

This ansible playbook uses https://github.com/Neo23x0/auditd for `audit.rules`
## Analyze `laurel` logs

After running the installation, you can find the processed `auditd` logs under `/var/log/laurel` directory
These are formatted as `JSON`, thus you can use `jq` to parse
```shell
sudo grep '"success":"no"' /var/log/audit.log | jq .
```

For more info check the repo: https://github.com/threathunters-io/laurel

## Firewall `ufw`

From the default UFW installation, there is just an extra SYN packets logging in the `after.rules`

```after.rules
-A ufw-after-input -p tcp --syn -j LOG --log-prefix "[UFW-SYN-LOG] "
```

If you want to add your own rules, refer to, for example:
- https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands
- https://orcacore.com/ufw-firewall-commands-rules/
- ...

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=ndanilo8/ansible-kali-build&type=Date)](https://star-history.com/#ndanilo8/ansible-kali-build&Date)

## Future releases

- [ ] Configure `BurpSuite` and Cert file for Firefox
- [ ] Firefox extensions: `FoxyProxy`, `Wappalyzer`, `Sidebery`...
- [ ] Custom Firefox Bookmarks + Folders
- [ ] Create new user and add to the `sudo` group + `NOPASSWD`
- [ ] Custom user profile picture, Login Screen & Desktop Wallpapers
- [ ] Customize top horizontal `xfce4-panel` with widgets (uptime, network monitor...)
