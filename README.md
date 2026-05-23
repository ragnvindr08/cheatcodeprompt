# Pro Debugger Terminal Cheatsheet
> Terminal & WSL only. No AI. Just the tools.

---

## Logs

| Command | What it does |
|---|---|
| `tail -f app.log` | Live-follow a log file in real time |
| `tail -f app.log \| grep ERROR` | Live log — errors only |
| `journalctl -u myapp -f --since today` | Follow systemd service logs from today |
| `journalctl -u myapp -n 100 --no-pager` | Last 100 lines of a systemd service |
| `grep -rn 'Exception' ./logs/ --include='*.log'` | Find exceptions across all log files |
| `awk '{print $1}' app.log \| sort \| uniq -c \| sort -rn \| head` | Count + rank most frequent log entries |

---

## Process debugging

| Command | What it does |
|---|---|
| `ps aux \| grep python` | Find running Python processes |
| `lsof -i :8080` | See what process is on port 8080 |
| `kill -9 $(lsof -t -i:8080)` | Kill whatever is using port 8080 |
| `strace -p 1234 -e trace=open,read,write` | Trace syscalls of a running process |
| `lsof -p 1234` | All files/sockets opened by a PID |
| `cat /proc/1234/environ \| tr '\0' '\n'` | Dump env vars of a running process |
| `pgrep -lf myapp` | Find PIDs matching a process name |

---

## Network debugging

| Command | What it does |
|---|---|
| `curl -v http://localhost:3000/api/health` | Verbose HTTP — headers, status, body |
| `curl -s -o /dev/null -w "%{http_code}" http://example.com` | Check HTTP status code only |
| `ss -tlnp` | All listening TCP ports and their processes |
| `netstat -an \| grep ESTABLISHED \| wc -l` | Count active connections |
| `tcpdump -i lo -A port 8080` | Sniff raw HTTP traffic on loopback |
| `wget --spider -S http://example.com 2>&1 \| head` | Check URL reachability without downloading |

---

## File inspection

| Command | What it does |
|---|---|
| `find . -name '*.py' -mmin -30` | Files modified in the last 30 minutes |
| `find . -size +10M -type f` | Find files larger than 10 MB |
| `du -sh * \| sort -rh \| head -10` | Top 10 largest files/dirs in current path |
| `diff <(ssh user@prod cat app.conf) app.conf` | Diff remote config vs local config |
| `xxd file.bin \| head -20` | Hex dump to inspect binary files |
| `inotifywait -m -r ./src -e modify` | Watch directory for file changes live |

---

## Performance

| Command | What it does |
|---|---|
| `htop` | Interactive CPU/memory/process monitor |
| `time python script.py` | Measure real, user, sys time of a command |
| `vmstat 1 10` | CPU, memory, I/O stats every 1 second |
| `iostat -xz 1` | Disk read/write throughput per device |
| `free -h && cat /proc/meminfo \| grep -i swap` | RAM and swap usage at a glance |
| `watch -n 1 "ps aux --sort=-%mem \| head -10"` | Live top-10 memory hogs, refresh every 1s |

---

## Git debugging

| Command | What it does |
|---|---|
| `git log --oneline --graph --all` | Visual branch history in terminal |
| `git bisect start && git bisect bad && git bisect good v1.0` | Binary search to find which commit broke it |
| `git diff HEAD~3 -- src/api.py` | Diff a specific file vs 3 commits ago |
| `git log -S "functionName" --source --all` | Find every commit that touched a string |
| `git stash show -p stash@{0}` | Inspect what's in a stash before applying |
| `git blame -L 50,70 src/main.py` | Who last changed lines 50–70 |

---

## Bash one-liners (bonus)

| Command | What it does |
|---|---|
| `sudo service docker start` | Start Docker daemon (WSL) |
| `sed -i 's/==$//' requirements.txt` | Strip trailing `==` from each line |
| `sed -i 's/old/new/g' file` | Replace all occurrences in a file |
| `sed -n '100,120p' file` | Print lines 100–120 only |
| `sed -i '657s/.*/new line content/' file` | Replace line 657 entirely |
| `grep -n "text" file` | Find lines with line numbers |
| `curl -o data.json https://example.com` | Download URL and save as file |

---

*No AI. No IDE. Just the terminal.*
