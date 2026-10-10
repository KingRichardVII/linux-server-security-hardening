# Implementation Notes

This is a concise summary of the actual exercised workflows, not a copy-paste installation script. Commands that alter access or storage should be reviewed for the target environment.

## Access controls

- Created users `alice` and `bob` and group `secops`.
- Added `alice` and `richard` to `secops`; Bob intentionally remained outside the group.
- Built `/srv/secops-share`, configured `root:secops` and `chmod 2770`.
- Verified group inheritance with new files and corrected one preexisting file with `chgrp secops`.
- Confirmed Bob received `Permission denied` and group members could collaborate.

## SSH

- Generated a passphrase-protected Ed25519 key on the Windows administration desktop.
- Transferred the *public* key into `~/.ssh/authorized_keys`; permissioned `~/.ssh` to `700` and `authorized_keys` to `600`.
- Added `01-homelab-hardening.conf` before the cloud-init drop-in.
- Validated with `sudo sshd -t` and `sudo sshd -T` before `systemctl reload ssh`.
- Tested key-only success and password-only refusal from independent PowerShell sessions.

## UFW

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from 192.168.0.0/24 to any port 22 proto tcp
sudo ufw enable
sudo ufw status verbose
```

The rule is scoped to the lab's IPv4 subnet. Do not reuse the address range blindly on another network. A Proxmox virtual console remained available as a recovery path.

## Storage

- Inspected `df -h /`, `lsblk -f`, `pvs`, `vgs`, and `lvs`.
- Extended the root LV twice with `sudo lvextend -L +5G /dev/ubuntu-vg/ubuntu-lv` (one unintended duplicate operation).
- Grew ext4 online with `sudo resize2fs /dev/ubuntu-vg/ubuntu-lv`.
- Verified root filesystem at approximately 25G and unallocated VG capacity at approximately 5G.

## Updates

- Refreshed APT indexes with `sudo apt update`.
- Simulated `apt-get -s upgrade` and `apt-get -s --with-new-pkgs upgrade`.
- Applied ordinary package upgrades and then installed required additional kernel packages using `apt-get --with-new-pkgs upgrade`.
- Booted kernel `7.0.0-38-generic` and performed the post-reboot security checks recorded in the README.
