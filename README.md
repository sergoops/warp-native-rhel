<p aling="center"><a href="https://github.com/distillium/warp-native">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="./media/logo.png" />
   <source media="(prefers-color-scheme: light)" srcset="./media/logo-black.png" />
   <img alt="Warp Native" src="./media/logo.png" />
 </picture>
</a></p>

**🇷🇺 [Русская версия](./README_ru.md)**

This script installs Cloudflare WARP in "native" mode via `WireGuard` as an interface, without using `warp-cli`.

⚠️ Supports **Debian/Ubuntu** and **RHEL-family** (AlmaLinux 10+, Rocky Linux, RHEL 8+, Fedora).

It automates:
- Installation of required packages
- Download and configuration of `wgcf`
- IPv6 availability check in the system
- Generation and modification of WireGuard configuration
- Connection and status verification
- Enable auto-start of `warp` interface

---

## 🚀 Installation Methods

### Option 1: Shell Script (Quick Install)

Install on each required node:

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/distillium/warp-native/main/install.sh)
```

### Option 2: Ansible Role (Recommended for automation)

For managing multiple servers, use the Ansible role:

**Install from Ansible Galaxy:**
```bash
ansible-galaxy install themelbine.warp_native
```

**GitHub Repository:** [ansible-role-warp-native](https://github.com/TheMelbine/ansible-role-warp-native)

**Example playbook:**
```yaml
- hosts: warp_servers
  become: yes
  roles:
    - themelbine.warp_native
  vars:
    warp_native_state: present
    warp_native_modify_resolv: true
```

## Supported Distributions

| Distribution | Minimum Version | Package Manager | Notes |
|-------------|----------------|-----------------|-------|
| AlmaLinux | 9+ | dnf | EPEL needed on AL9, base repos on AL10+ |
| Rocky Linux | 9+ | dnf | May require EPEL on older versions |
| RHEL | 8+ | dnf | May require EPEL on older versions |
| Fedora | 38+ | dnf | `wireguard-tools` in base repos |
| Debian | 11+ | apt | `wireguard` meta-package |
| Ubuntu | 22.04+ | apt | `wireguard` meta-package |

> On RHEL 8/9 and older Rocky Linux, the script automatically installs EPEL if `wireguard-tools` is not available in enabled repos. AlmaLinux 10+ includes it in base repos.

## Xray Configuration Templates

<details>
  <summary>📝 Show outbound example</summary>

```json
{
  "tag": "warp-out",
  "protocol": "freedom",
  "settings": {
    "domainStrategy": "UseIP"
  },
  "streamSettings": {
    "sockopt": {
      "interface": "warp",
      "tcpFastOpen": true
    }
  }
}
```
</details>

<details>
  <summary>📝 Show routing rule example</summary>

```json
{
  "type": "field",
  "domain": [
    "netflix.com",
    "youtube.com",
    "twitter.com"
  ],
  "inboundTag": [
    "Node-1",
    "Node-2"
  ],
  "outboundTag": "warp-out"
}

```
</details>

## WARP Interface Management

| Operation                    | Command                             |
|------------------------------|-------------------------------------|
| Check service status         | `systemctl status wg-quick@warp`    |
| Show information (wg)        | `wg show warp`                      |
| Stop interface               | `systemctl stop wg-quick@warp`      |
| Start interface              | `systemctl start wg-quick@warp`     |
| Restart interface            | `systemctl restart wg-quick@warp`   |
| Disable auto-start           | `systemctl disable wg-quick@warp`   |
| Enable auto-start            | `systemctl enable wg-quick@warp`    |

## Uninstall

### Shell Script Method:
```bash
bash <(curl -fsSL https://raw.githubusercontent.com/distillium/warp-native/main/uninstall.sh)
```

### Ansible Method:
```yaml
- hosts: warp_servers
  become: yes
  roles:
    - themelbine.warp_native
  vars:
    warp_native_state: absent
```

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

Created by [distillium](https://github.com/distillium)

## Language Support

The installation script supports interactive language selection. During installation, you'll be prompted to choose between English and Russian.
