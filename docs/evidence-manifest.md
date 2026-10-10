# Publication Evidence Manifest

These screenshots are **sanitized derivatives of actual Lab 01 terminal/Proxmox captures**. Unmodified originals are retained outside this public repository in private lab documentation. Screenshots are evidence of observed exercises and are **not** recreated terminal output.

| Published screenshot | Purpose | Privacy treatment |
| --- | --- | --- |
| `infrastructure-proxmox-linux01.png` | VM summary, running Ubuntu guest | VM endpoint IP and link-local IPv6 address masked |
| `linux-setgid-directory-before-after.png` | Setgid mode `770` to `2770` | No endpoint data visible |
| `linux-unauthorized-access-denied.png` | Unauthorized Bob account denied | Generic lab accounts remain |
| `ssh-effective-hardening-settings.png` | Effective `sshd -T` values | No endpoint data visible |
| `ssh-public-key-allowed.png` | Ed25519 key-authenticated SSH success | Windows profile/key path and endpoint IPs masked; login banner omitted with visible disclosure |
| `ssh-password-only-denied.png` | Password-only method rejected | Windows profile path and destination endpoint IP masked |
| `ssh-failed-login-journal.png` | Failed authentication event in systemd journal | Source endpoint IP masked; timestamp, service, account, and failure result preserved |
| `ufw-active-policy.png` | UFW activation, default deny, subnet SSH allowance | Individual endpoint IPs masked; `192.168.0.0/24` intentionally preserved |
| `ufw-blocked-igmp-events.png` | UFW blocked multicast IGMP traffic | MAC field and router source IP masked; event and multicast details preserved |
| `lvm-before.png` | Initial LV/root filesystem and VG free space | No endpoint data visible |
| `lvm-after.png` | LV and ext4 filesystem expansion | No endpoint data visible |
| `post-kernel-upgrade-validation.png` | Kernel, services, UFW, SSH, storage verification | No exact endpoints visible; subnet rule remains |

## Release checklist

- [x] Keep unmodified original evidence **out of this repository**.
- [x] Apply solid, opaque masks to unnecessary workstation profiles, specific endpoint IPs, MAC-like identifiers and visible link-local IPv6 data.
- [x] Preserve actual configuration values necessary to understand the tested security controls, including the CIDR-scoped SSH rule.
- [x] Remove unnecessary original capture filenames and capture-archive references from the public manifest.
- [x] Re-encode screenshots as PNGs without inherited metadata.
- [x] Keep private key material, passwords, passphrases, and API tokens out of the repository.
- [ ] **Human review:** Open all 12 final screenshots at full resolution and check for missed sensitive details.
- [ ] **Local pre-commit check:** Inspect staged files and their diff; ensure no extra files or `.git` history with old screenshots enter a public commit.
- [ ] **GitHub preview:** Verify tables, images, architecture Mermaid diagram, and mobile readability after upload.

This archive is a **review candidate**, not evidence that the repository is public or that all possible identifiers have been found.
