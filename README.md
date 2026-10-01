# Linux Security Hardening

Hands-on hardening of an Ubuntu 22.04.5 LTS server for a fictional client, **Baker Street Corporation** (host: `Baker_Street_Linux_Server`), completed in a lab environment as part of the Columbia University Cybersecurity Boot Camp.

## Summary
- Audited users and groups, and configured sudo permissions
- Secured SSH and enforced password policies
- Enabled system logging and automated recurring administrative tasks with cron
- Applied least-privilege and system-hardening principles

## Environment
| Item | Detail |
|---|---|
| OS | Ubuntu 22.04.5 LTS |
| Hostname | Baker_Street_Linux_Server |
| Memory | 15 GiB total, no swap |

## Tasks and commands
| Area | What was done | Commands / files |
|---|---|---|
| OS backup | Full-system compressed backup before changes | `sudo tar -cvpzf /baker_street_backup.tar.gz --exclude=/baker_street_backup.tar.gz --exclude=/proc --exclude=/tmp --exclude=/mnt --exclude=/sys --exclude=/dev --exclude=/run /` |
| Users & groups | Removed terminated users, locked users on temporary leave, checked group membership (no users in the marketing group) | `sudo userdel --remove <user>`, `sudo usermod -L <user>`, `manage_marketing.sh` |
| Password policy | Installed and configured PAM password quality; tested with a test user | `sudo apt install libpam-pwquality`, `/etc/pam.d/common-password` |
| Sudo permissions | Reviewed and updated sudoers | `sudo visudo` |
| File permissions | Removed "other" access from files; set group-restricted access on finance, engineering and research files | `find /home -type f -perm /007 -exec chmod o-rwx {} \;`, `find /home -type f -iname "*finance*" -exec chmod 770 {} \;` (same for `*engineering*`, `*research*`) |
| SSH | Audited and secured SSH configuration, restarted service | `/etc/ssh/sshd_config`, `service ssh restart` |
| Packages | Updated system, removed insecure packages (telnet, rsh-client), installed lynis, ufw and tripwire | `apt update`, `apt upgrade -y`, `apt autoremove -y`, `apt list --installed >> package_list.txt` |
| Services | Reviewed services and removed unnecessary ones (e.g. samba, mysql) | `service --status-all`, `apt remove --purge` |
| Logging | Configured log rotation and persistent journald logging | `/etc/logrotate.conf`, `/etc/systemd/journald.conf` |
| Scripts | Wrote hardening/system-info scripts that log configuration data to a report file | `hardening_script.sh`, `system_info.sh` |
| Cron | Scheduled the scripts to run automatically | `crontab -e` |

## Conclusion
- Password policies needed updating and enforcing to make account compromise harder.
- Services with known vulnerabilities were removed and security packages (Lynis, UFW, Tripwire) added to improve integrity and auditing.
- Users, groups, files and directories were audited so access is limited to what each group and user requires.

## Documentation
Full checklist with screenshots: [`docs/Project1_Hardening_Summary_and_Checklist.pdf`](docs/Project1_Hardening_Summary_and_Checklist.pdf)

## Skills demonstrated
Linux administration, least privilege, SSH hardening, PAM password policy, file permissions, log management, cron automation, Lynis/UFW/Tripwire.

> Lab exercise on an isolated training environment. Hashes and screenshots come from disposable lab systems.
