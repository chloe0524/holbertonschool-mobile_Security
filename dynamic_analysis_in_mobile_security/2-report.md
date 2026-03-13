# Apk_task2 - Traffic Interception Report

## Goal
Intercept application HTTPS traffic and recover the hidden flag.

## 1) Install app and configure proxy
```bash
irydia@ChloeDell:~$ adb -s emulator-5554 install app-release-task2.apk
irydia@ChloeDell:~$ adb shell settings put global http_proxy 10.5.0.22:8080
```

## 2) Prepare Burp certificate for Android
```bash
irydia@ChloeDell:~$ openssl x509 -inform DER -in certificate.der -out burp_cert.pem
irydia@ChloeDell:~$ openssl x509 -inform PEM -subject_hash_old -in burp_cert.pem | head -1
9a5ba575
irydia@ChloeDell:~$ mv burp_cert.pem 9a5ba575.0
irydia@ChloeDell:~$ adb push 9a5ba575.0 /sdcard
9a5ba575.0: 1 file pushed. 0.1 MB/s (1326 bytes in 0.025s)
```

## 3) Extract the flag
After importing the certificate on the emulator and relaunching the app, traffic was visible through Burp. The response containedflag.

## Decrypted flag
`Holberton{keystore_is_not_as_safe_as_u_think!}`
