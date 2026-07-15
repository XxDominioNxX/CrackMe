### Keyboard cache
Клавиатура может сохранять введенные данные в словарь, если input был не правильно сконфигурирован при разработке приложения.
Словари можно найти тут:
 - /data/data/com.android.inputmethod.latin/databases/
 - /data/data/com.google.android.inputmethod.latin/files/personal/userhistory/


### Clipboard - copy and paste
Копирование производится в глобальный буфер. Вредоносные приложения могут записывать данные из него.



### Insecure Logging
Логируются важные данные (токены, логины, пароли, секреты и т.п.). 
Другие приложения с разрешением 
READ_LOGS или пользователи с adb могут получить к ним доступ.

```
adb logcat
```

<img width="1011" height="113" alt="image" src="https://github.com/user-attachments/assets/a38fe39f-d4c8-483e-86d6-e249b4338afb" />
