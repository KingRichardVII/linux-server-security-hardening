# Troubleshooting and Lessons Learned

The three primary case-study examples are **stale supplementary-group credentials**, **SSH configuration precedence**, and **unintended extra LVM allocation**. The remaining entries document supporting controlled experiments and evidence-based log triage. No exercise here is represented as an unexpected production incident.

## 1. Group membership not reflected in an existing login

**Observed:** `getent group secops` listed Richard, but `id` in an old SSH session did not include the group, and access was denied.

**Cause:** Supplementary groups were established when the login session started. Changing the account database did not automatically update those process credentials.

**Resolution:** Start a new SSH login; verify with `id`; retest the access policy.

## 2. Setgid directory did not change previously created file groups

**Observed:** A newly created Alice file inherited `secops`, while an earlier file remained `alice:alice`.

**Cause:** Directory setgid affects future file creation, not retroactively stored ownership.

**Resolution:** `chgrp secops` on the earlier file, with appropriate ownership/group membership. Re-tested cross-user writes.

## 3. Accidental extra LVM extension

**Observed:** Intended 5G growth was repeated, increasing `ubuntu-lv` from approximately 15G to approximately 25G while ext4 still showed 15G.

**Diagnosis:** `lvs`, `vgs`, and `df -h /` distinguished LV allocation from filesystem size. A speculative `lvextend -L -5G` attempt was rejected and did not shrink the volume.

**Resolution:** Avoided an unnecessary live-root reduction; used `resize2fs` to grow ext4 to the existing 25G LV. Confirmed capacity and kept ~5G VG unallocated.

## 4. Chrony controlled administrative stop versus service failure

**Observed:** During an intentionally induced outage, `systemctl status chrony` showed `inactive (dead)` while enabled; `pgrep` returned no daemon instances.

**Investigation:** `journalctl -u chrony` recorded successful deactivation and stopping; the administrator had intentionally issued `systemctl stop chrony`.

**Resolution:** Started chrony, checked new processes and `chronyc tracking`, and verified normal time synchronization.

## 5. Benign-looking operational impact versus alarming log messages

**Observed:** `KHO: Failed to reserve lowmem scratch buffer` during boot.

**Triage:** Server reachable, zero failed units and no demonstrated dependent failure. No configuration changes made without evidence of impact.

## 6. SSH policy precedence (primary security configuration case)

**Observed:** `/etc/ssh/sshd_config.d/50-cloud-init.conf` set `PasswordAuthentication yes`.

**Resolution:** Introduced earlier `01-homelab-hardening.conf`, validated the **effective** settings with `sshd -T`, reloaded, and separately tested ALLOW/DENY behavior.

## Takeaway

The key lesson was to separate **configured intent**, **effective/runtime state**, and **verified behavior**. Group authorization, SSH authentication, and LVM growth each required reading more than one source of evidence before taking action. For safety, configuration changes were tested using independent sessions or tools before removing recovery access.
