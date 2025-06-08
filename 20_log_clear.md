# 📝 Log & Command History Management (Linux & Windows)

## 🔹 Linux (Using Terminal / Shell)

### ✅ View Logs
Linux logs are mostly stored in `/var/log/`. Use the following commands:

| Command                        | Purpose                             |
|-------------------------------|-------------------------------------|
| `cat /var/log/syslog`         | Show full system log (Debian-based) |
| `cat /var/log/messages`       | Show full system log (RedHat-based) |
| `tail -f /var/log/syslog`     | Live system log (real-time view)    |
| `less /var/log/auth.log`      | View authentication logs            |
| `journalctl`                  | View journal logs (systemd systems) |

### 🧹 Clear Logs
Requires root permissions:

```bash
sudo truncate -s 0 /var/log/syslog
sudo truncate -s 0 /var/log/auth.log
sudo truncate -s 0 /var/log/messages

# Clear journal logs (systemd)
sudo journalctl --rotate
sudo journalctl --vacuum-time=1s
```

### 🧽 Clear Command Line History
```bash
# Clear current session history
history -c

# Also remove history file
rm ~/.bash_history
```


## 🔹 Windows (Using Command Prompt - CMD)

### ✅ View Logs
Open Event Viewer using:

```cmd
eventvwr
```

Or use PowerShell:
```cmd
powershell "Get-EventLog -LogName System -Newest 20"
```

### 🧹 Clear Logs
Using CMD (admin rights required):
```cmd
wevtutil cl System
wevtutil cl Application
wevtutil cl Security
```

Using PowerShell:
```powershell
Get-EventLog -LogName * | ForEach { Clear-EventLog -LogName $_.Log }
```

### 🧽 Clear Command Line History
```cmd
# CMD does not retain history after closing the session
# To clear current buffer (visible with F7)
doskey /listsize=0
```

> Note: PowerShell has a history file (`ConsoleHost_history.txt`) that can be cleared manually:
```powershell
Remove-Item (Get-PSReadlineOption).HistorySavePath
```
