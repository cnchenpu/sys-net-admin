# SELinux

1. Enforced : Actions contrary to the policy are blocked and a corresponding event is logged in the audit log.
2. Permissive : Actions contrary to the policy are only logged in the audit log.
3. Disabled : The SELinux is disabled entirely.

SELinux configuration file ```/etc/selinux/config```:
```bash
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

Verify the current mode of SELinux:
```bash
$ getenforce
$ sestatus
```

To switch between the SELinux modes temporarily:
```bash
$ setenforce Enforcing | Permissive | 1 | 0
```
