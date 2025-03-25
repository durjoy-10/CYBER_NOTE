# Crack ZIP Passwords with John the Ripper (Kali Linux)

## 1. Create a Password-Protected ZIP File
```bash
zip -e p.zip file.txt  # -e = encrypt, p.zip = output, file.txt = file to zip  
# (Enter a password when prompted.)
```

## 2. Extract the Hash for Cracking
```bash
zip2john p.zip > hash.txt  # Saves the hash in hash.txt  
```

## 3. Crack the Password

### First Try (Default Attack)
```bash
john hash.txt  
# If successful, the password is displayed.
# But if I use it second time then password is not shown.
```

### Show Password Again
```bash
john hash.txt --wordlist="/usr/share/wordlist"
```
```bash
john --show hash.txt  #Now it shows the password of the zip file again
```


## 4. Unzip the File
```bash
unzip p.zip  # Enter the cracked password  
```

## Summary
- `zip -e` → Create encrypted ZIP.
- `zip2john` → Extract hash.
- `john` → Crack password (default/wordlist).
- `unzip` → Open the file.
