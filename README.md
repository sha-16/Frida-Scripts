# Frida-Scripts
Dos scripts que me salvaron la vida de cara a un pentest mobile, para bypassear un SSL Pinning durísimo en conjunto con Anti-Root! Los cargan con Frida cualquier cosa!

```
adb shell settings put global http_proxy localhost:3333
adb shell "iptables -t nat -F"
adb shell "iptables -t nat -A OUTPUT -p tcp --dport 443 -j DNAT --to-destination 127.0.0.1:3333
adb shell "iptables -t nat -A POSTROUTING -p tcp --dport 443 -j MASQUERADE"
adb reverse tcp:3333 tcp:8080

adb shell "/data/local/tmp/frida-server &"

frida -U -f com.tu.app -l disable-flutter-tls.js -l rootbeer_bypass.js
```

Si vas más a fondo y quieres modificar un SMALI, puedes meter esto y mandar todas las protecciones al `/dev/null`.

```
.method public static final checkRootMethod1()Z
    .locals 1

    const/4 v0, 0x0

    return v0
.end method
```
