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



