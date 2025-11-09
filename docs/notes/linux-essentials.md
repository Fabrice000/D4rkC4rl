# Linux Essentials — Practical Cheatsheet & Notes

> “A machine whispers when you learn its dialect.”  
> Short, actionable notes for the terminal-bound mind.

---

## 🧭 Quick Philosophy
Linux is a toolset, not a religion. Learn to read logs, chain small commands, and automate. Always prefer understanding over memorizing.

---

## 🔑 Useful Concepts
- **The filesystem**: `/etc` (config), `/var` (logs, runtime), `/home` (users), `/root` (admin).  
- **Permissions**: `rwx` for user/group/others. `chmod`, `chown`.  
- **Processes & signals**: `ps`, `top`/`htop`, `kill -9 PID`, `pkill`, `nice`/`renice`.  
- **Services**: `systemctl start/stop/status` (systemd).  
- **Networking**: `ip`, `ss`, `netstat` (legacy), `iptables`/`nftables`.

---

## 🧰 Essential Commands (with intent + example)

### File & text
- `ls -la` — list all, including hidden files.  
- `file target` — detect file type.  
- `cat file`, `less file`, `head -n 50 file`, `tail -f file` — view files.  
- `grep -R --line-number "needle" /path` — recursive search with line numbers.  
- `sed -n '1,200p' file` — print specific range.  
- `awk '{print $1,$3}' file` — quick column extraction.

```bash
# find all config files mentioning "password"
grep -R --line-number -i "password" /etc 2>/dev/null
