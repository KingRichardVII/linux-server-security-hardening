# Security Controls and Limitations

| Control | Configuration | Validation | Limitations |
| --- | --- | --- | --- |
| Group-based access | `root:secops`, directory mode `2770` | Alice/Richard access, Bob denied | This is POSIX group authorization, not enterprise centralized RBAC. |
| SSH authentication | Key-based login enabled; password-only and direct root login disabled | Key-only session accepted; password-only session rejected | Client private-key compromise and sudo privilege remain important risks. |
| Host firewall | UFW default-deny inbound; TCP/22 allowed from `192.168.0.0/24` | UFW active; new allowed SSH session worked | No controlled outside-subnet TCP negative test; successful LAN SSH and observed IGMP blocks do not prove this separate condition. The local subnet is broader than a single workstation. |
| IPv6 | UFW `IPV6=yes`, default-deny inbound | Configuration inspected | A separate external IPv6 ingress test was not performed. |
| Logging | systemd journal | Investigated controlled failed SSH authentication | No centralized retention/alerting in Lab 01. |
| Patching | APT preview, updates, kernel reboot | Kernel and services checked after reboot | Five phased updates remained pending at snapshot time. |

**Interpretation caution:** A `UFW BLOCK` log line showing IPv4 multicast/IGMP traffic from the router proves some traffic was blocked; it does not prove malicious intent or that all unauthorized TCP flows were tested.

**SSH interpretation caution:** The SSH failed-password incident predated password-authentication hardening. Log entries showing accepted passwords from an earlier timestamp do not establish the newer hardened configuration is bypassed.

**Recovery note:** The Proxmox snapshot created before patching supported short-term rollback planning but was not a substitute for a tested backup. Chrony shutdown/startup was a controlled exercise, not a real service incident.
