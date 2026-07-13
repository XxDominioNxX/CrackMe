- Попытка ввода случайного ключа
<img width="506" height="522" alt="image" src="https://github.com/user-attachments/assets/d8e16461-4fc2-4172-a438-8e1bc92c802d" />

- Открыл .apk в jadx
- В MainActivity проверка вводимого значения выполняется в классе CodeCheck
<img width="396" height="204" alt="image" src="https://github.com/user-attachments/assets/017d2db6-7cbd-4b7e-9ff5-542921fcc488" />

- Функция bar объявлена с модификатором native. Это означает, что её реализация написана не на Java, а на языке C/C++ и находится в нативной библиотеке (.so файле), которая подгружается в MainActivity
<img width="1017" height="342" alt="image" src="https://github.com/user-attachments/assets/df45450e-275a-45e9-91d2-8fe826d22d5c" />

- Распаковал .apk как архив. Нашел библиотеку в по пути /lib/x86/libfoo.so
- Открыл libfoo.so в Ida Pro
- В окне функций нашел подходящую по имени Java_sg_vantagepoint_uncrackable2_CodeCheck_bar
<img width="427" height="117" alt="image" src="https://github.com/user-attachments/assets/b8fba010-96d4-4d4a-a901-7bccddc7119f" />

Вот ее содержимое полученное через Hex-Rays:
<img width="950" height="512" alt="image" src="https://github.com/user-attachments/assets/69521007-5c75-40ae-a0c6-05c42afea54f" />


- Видно сравнение длины входного аргумента со значениемм 23 (Предположительно длина)
- Далее сравнение входного аргумента strncmp со значениями в переменных типа int
- Изменил тип s2 (Начало данных) c "int s2" на "char s2 [23]"
<img width="736" height="402" alt="image" src="https://github.com/user-attachments/assets/f67bdf67-154a-4b21-9a19-0ae546e36f2e" />

- Вот вид функции после редактирования:
<img width="907" height="416" alt="image" src="https://github.com/user-attachments/assets/de689e5a-c4f6-4caa-873a-3d43ae41c78b" />

- Флаг "Thanks for all the fish"!
- Проверка в эмуляторе
<img width="540" height="537" alt="image" src="https://github.com/user-attachments/assets/c794abbe-e7a9-44ae-9866-79328084affa" />


