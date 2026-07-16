## XSS
- Открыл в jadx
  Нужный класс выглядит так:
  <img width="794" height="287" alt="image" src="https://github.com/user-attachments/assets/2c7d99c5-986b-4f17-9624-90112206354c" />

- Используется WebView с разрешением выполнения js кода
- Проверка базового:
  ```
  <img src=x onerror=alert('XSS')>
  <h1 style="color: red; background: yellow;">XSS УСПЕШЕН!</h1>
  ```
  <img width="426" height="296" alt="image" src="https://github.com/user-attachments/assets/b275e405-fb67-48d9-9fda-67dba3045b11" />



### SQL
- Открыл в jadx
- Нашел место, где входные данные подставляются сразу в строку Sql-запроса без экранирования и параметров
<img width="1111" height="102" alt="image" src="https://github.com/user-attachments/assets/e842c62b-503a-4e2e-a314-77d932ec1212" />

- Пробуем написать injection:
```
' or 1=1 --
```

- По уведомлению видим, что данные подставляются успешно
<img width="374" height="298" alt="image" src="https://github.com/user-attachments/assets/0735e041-9c1f-45e4-a3c3-250994fbf22a" />



### Web View
- Нашел в jadx нужный участок кода
<img width="795" height="234" alt="image" src="https://github.com/user-attachments/assets/f4628adc-7244-4203-a25f-2074eb66196c" />
- Видим множество разрешений. Например на просмотр файловой системы из url
- Составляем url на просмотр какого-нибудь файла:
  ```
file:///etc/audio_effects.conf
  ```

- Итог:
<img width="406" height="518" alt="image" src="https://github.com/user-attachments/assets/25631c70-d1a1-4e11-ab03-57b0b3c6f96a" />

