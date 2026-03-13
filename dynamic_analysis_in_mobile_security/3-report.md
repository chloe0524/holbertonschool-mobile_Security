# Apk_task3

## 1) Setup
```bash
irydia@ChloeDell:~$ adb -s emulator-5554 install task3_d.apk
irydia@ChloeDell:~$ adb push frida-server-17.5.1-android-x86_64 /data/local/tmp/
irydia@ChloeDell:~$ adb shell chmod 755 /data/local/tmp/frida-server
irydia@ChloeDell:~$ adb shell /data/local/tmp/frida-server &
```

## 2) Find useful classes
I first listed loaded classes containing `holberton`.

`list_classes.js`
```javascript
Java.perform(() => {
    Java.enumerateLoadedClasses({
        onMatch: function (className) {
            if (className.includes("holberton")) console.log(className);
        },
        onComplete: function () {
            console.log("Done listing classes.");
        }
    });
});
```

```bash
irydia@ChloeDell:~$ frida -U -p 21668 -l list_classes.js
```

Relevant classes included:
- `com.holberton.task4_d.MainActivity`
- `com.holberton.task4_d.MainActivityKt`
- `com.holberton.task4_d.MainActivity$retrieveEncryptedData$1`

## 3) Inspect methods and locate hidden routine
I dumped methods from `MainActivity$retrieveEncryptedData$1`:

```bash
irydia@ChloeDell:~$ frida -U -p 27890 -l dump_methods.js
```

Then in JADX, ther is suspicious private static method in `MainActivityKt`:
`aBcDeFgHiJkLmNoPqRsTuVwXyZ123456(Function1)`.

## 4) Call the hidden method manually
With `flag.js`, I used reflection to:
1. locate the private method,
2. set it accessible,
3. invoke it with custom `Function1` callback that prints the decoded value.

```bash
irydia@ChloeDell:~$ frida -U -p 27890 -l flag.js
```

Frida output:
`Holberton{calling_uncalled_functions_is_now_known!}`

## Decrypted flag
`Holberton{calling_uncalled_functions_is_now_known!}`

