# John the Ripper Password Cracking Cheat Sheet

## 1. Cracking Linux (/etc/shadow) Passwords

### Extract hashes:
```sh
sudo unshadow /etc/passwd /etc/shadow > linux_hashes.txt
```

### Crack SHA-512 hashes:
```sh
john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt linux_hashes.txt
```

### View results:
```sh
john --show linux_hashes.txt
```

## 2. Cracking Windows (NTLM) Passwords

### Example hash file (nt_hashes.txt):
```
Administrator:500:AAD3B...:31D6CFE0D16AE931B73C59D7E0C089C0:::
```

### Crack command:
```sh
john --format=nt --wordlist=rockyou.txt nt_hashes.txt
```

### Show cracked passwords:
```sh
john --show --format=nt nt_hashes.txt
```

## 3. Cracking ZIP Archives

### Extract hash:
```sh
zip2john file.zip > zip_hash.txt
```

### Crack password:
```sh
john --wordlist=rockyou.txt zip_hash.txt
```

### View results:
```sh
john --show zip_hash.txt
```

## 4. Cracking RAR Archives

### Extract hash:
```sh
rar2john file.rar > rar_hash.txt
```

### Crack password:
```sh
john --wordlist=rockyou.txt rar_hash.txt
```

### View results:
```sh
john --show rar_hash.txt
```

## 5. Cracking SSH Private Keys

### Extract hash:
```sh
ssh2john id_rsa > ssh_hash.txt
```

### Crack passphrase:
```sh
john --wordlist=rockyou.txt ssh_hash.txt
```

### View results:
```sh
john --show ssh_hash.txt
```

## Advanced Techniques

### Use mutation rules:
```sh
john --wordlist=rockyou.txt --rules nt_hashes.txt
```

### Brute-force attack:
```sh
john --incremental=Alnum target_hashes.txt
```

### Multi-core processing:
```sh
john --fork=4 target_hashes.txt
```

### Resume interrupted session:
```sh
john --restore=last_session
```

## Common Commands Reference

| Task | Command |
|------|---------|
| Linux hashes | `john --format=sha512crypt --wordlist=rockyou.txt hashes.txt` |
| Windows NTLM | `john --format=nt --wordlist=rockyou.txt nt_hashes.txt` |
| ZIP files | `zip2john file.zip > zip_hash.txt && john --wordlist=rockyou.txt zip_hash.txt` |
| RAR files | `rar2john file.rar > rar_hash.txt && john --wordlist=rockyou.txt rar_hash.txt` |
| SSH keys | `ssh2john id_rsa > ssh_hash.txt && john --wordlist=rockyou.txt ssh_hash.txt` |
