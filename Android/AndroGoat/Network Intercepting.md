## HTTP
Для перехвата трафика с устройства использую Burp Suite
- Настраиваю прокси в Burp Suite на все интерфейсы и порт 8082 (На ваше усмотрение)
<img width="1267" height="708" alt="image" src="https://github.com/user-attachments/assets/4ebd4d78-c9e2-4e16-8a28-72b9c3686ff3" />

- Указываю прокси в эмуляторе
<img width="811" height="634" alt="image" src="https://github.com/user-attachments/assets/930e23c1-7a13-48a0-982b-b643b9971d80" />

- Видим запрос, которое делает приложение
<img width="1644" height="887" alt="image" src="https://github.com/user-attachments/assets/3ca27089-aa8c-482e-8de9-0d70938c9943" />



## HTTPS
- Для промотра Https-трафика, в добавок к пунктам Http, нужно испольуемый burp suite сертификат сделать доверенным
- Сам сертификат экспортирую из burp suite. Название любое, расширение файла .cer
<img width="931" height="672" alt="image" src="https://github.com/user-attachments/assets/013824bc-82eb-4722-a038-a27ab7c3e5d2" />

- Копирую его на устройство. Добавляю в доверенные: Settings -> Security -> Encryption & credentials -> Install a certificate
- Пробую перехватить Https-трафик
<img width="1642" height="765" alt="image" src="https://github.com/user-attachments/assets/5b788fbe-4ae7-4441-a539-81a66b0f6f06" />



# Certificate pinning - OKHTTP3
- Даже если система доверяет сертификату Burp, библиотека OkHttp3 внутри приложения "знает", как должен выглядеть настоящий сертификат сервера (или его хэш). Видя сертификат Burp, она обрывает соединение.
<img width="1109" height="129" alt="image" src="https://github.com/user-attachments/assets/7964e605-4e53-4dae-82e2-dde57eea6331" />

- Использую средство Objection для решение этой задачи
```
pip install objection
objection -g owasp.sat.agoat explore
android sslpinning disable
```
<img width="1017" height="454" alt="image" src="https://github.com/user-attachments/assets/6fe0a645-189a-4a00-90f1-1694ebd32206" />

- Пробую перехватить трафик
<img width="1641" height="859" alt="image" src="https://github.com/user-attachments/assets/64e8b1c6-7137-44df-804f-77c7622d0472" />


#### Дополнительно:
Можно использовать Frida напрямую:

Команда:
```
frida -U -l ssl_bypass.js -f owasp.sat.agoat
```

ssl_bypass.js
```
console.log("[*] Загрузка скрипта обхода SSL Pinning (OkHttp3)...");

Java.perform(function () {
    console.log("[*] Хукаем okhttp3.CertificatePinner.check()");
    
    try {
        var CertificatePinner = Java.use('okhttp3.CertificatePinner');
        CertificatePinner.check.overload('java.lang.String', 'java.util.List').implementation = function (str, list) {
            console.log("[+] Обход проверки сертификата для хоста: " + str);
            return; 
        };
        console.log("[*] Успешно захукали okhttp3!");
    } catch (err) {
        console.log("[-] Не удалось найти okhttp3.CertificatePinner: " + err);
    }

    // Дополнительный хук для TrustManager
    try {
        var TrustManagerImpl = Java.use('com.android.org.conscrypt.TrustManagerImpl');
        TrustManagerImpl.checkTrustedRecursive.implementation = function() {
            console.log("[+] Обход TrustManagerImpl.checkTrustedRecursive");
            return java.util.Collections.emptyList();
        };
    } catch (err) { }
});
```


# Certificate pinning - NATIVE
- Objection умеет обходить нативный pinning:
<img width="1650" height="487" alt="image" src="https://github.com/user-attachments/assets/d549f279-8942-4ad0-94a3-dd4ef9a0eccb" />
