# Apk_task1

## 1) Setup
```bash
irydia@ChloeDell:~$ adb -s emulator-5554 install task1_d.apk
irydia@ChloeDell:~$ adb push frida-server-17.5.1-android-x86_64 /data/local/tmp/
irydia@ChloeDell:~$ adb shell chmod 755 /data/local/tmp/frida-server
irydia@ChloeDell:~$ adb shell /data/local/tmp/frida-server &
```

## 2) Identifyhook point
From JADX/Android Studio, `MainActivity.getSecretMessage()` is tied to `libnative-lib.so`.
Instead of reversing the whole `.so`, I hooked this Java method and captured its runtime return value.

## 3) Hook script (`script.js`)
```javascript
Java.perform(function () {
    var MainActivity = Java.use("com.holberton.task2_d.MainActivity");

    MainActivity.getSecretMessage.implementation = function () {
        var result = this.getSecretMessage();

        if (result && result.indexOf("Holberton{") !== -1) {
            console.log("\n=====FLAG FOUND=====");
            console.log(result);
            console.log("======================\n");
        }

        return result;
    };
});
```

## 4) Run and extract
```bash
irydia@ChloeDell:~$ frida -U -f com.holberton.task2_d -l script.js
```
After pressing app button, Frida printed:
`Holberton{native_hooking_is_no_different_at_all}`

## Decrypted flag
`Holberton{native_hooking_is_no_different_at_all}`

