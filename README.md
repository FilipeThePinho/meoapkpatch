# meoapkpatch

release apk 4.11.1 (2023-12-02 : Merry Christmas guys!) ready to run: https://github.com/FilipeThePinho/meoapkpatch/releases/download/4.11.1/MEO_4.11.1.apk

note: you have to install it via ADB. First enable USB debug and you can install over the network via ./adb connect IP; ./adb install -r file.apk

## decompile

use apktool to decompile
```shell
apktool d meo.apk
```

## apply patch

read the patch file and change yourself or apply it using patch. patch file expects the folder to name MEO_4.8.0 - created based on this version
```shell
patch -p1 < meopatch.patch
```
if you see "different line endings" error, just:
```shell
dos2unix meopatch.patch
patch -p1 < meopatch.patch
```
accept the changes.

## build using apktool
```shell
apktool b -f -v meo -o meo-patched.apk
```
## align in 4-byte (required for newer API levels and SDKs)

```shell
zipalign -f -p 4 meo-patched.apk meo-patched-zipalign.apk
```
(exists on android SDK)

## sign using apksigner (jarsign doesn't work with 4-byte alignment)
```shell
apksigner sign --verbose --ks meopatch.keystore meo-patched-zipalign.apk
```
- input your keystore password to unlock and uses the pki
- you can create a keystore using keytool (found on JDK bin folder):
```shell
keytool -genkey -v -keystore meopatch.keystore -alias meopatch -keyalg RSA -keysize 2048 -validity 10000
```


(exists on android SDK)
meo-patched-zipalign.apk is now signed and ready to install

install meo-patched-zipalign.apk via adb (enable USB debug via developer mode - tap 10x build name on settings -, find the IP address, then:
```shell
./adb connect <IP>
./adb install -r meo-patched-zipalign.apk
```
Optional but recommended: disable USB debug and develop mode
