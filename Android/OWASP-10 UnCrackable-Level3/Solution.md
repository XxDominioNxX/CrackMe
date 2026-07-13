-	Запуск производился на эмуляторе с root-правами
-	Открыл .apk в jadx
-	Исследуя код, выявлены следующие методы защиты:
MainActivity: 
verifyLibs() – проверка целостности загружаемых библиотек и classes.dex
onCreate() – isDebuggerConnected, RootDetection

-	Так же в MainActivity найдена строка, которая скорее всего пригодится: 
private static final String xorkey = "pizzapizzapizzapizzapizz";

-	При провале любой проверки выполняется ShowDialog и приложение закрывается
-	Патч .apk
<img width="766" height="183" alt="image" src="https://github.com/user-attachments/assets/6d73b13f-e466-4744-8616-2f15e53b530d" />
-	Теперь можно искать флаг. Проверка вводимого значения производится в классе CodeCheck -> bar (native из libfoo.so)
-	Расспаковал .apk, нашел libfoo.so для нужной в моем случае архитектуры и открыл в Ida Pro
-	Найдена функция проверки bar
<img width="673" height="122" alt="image" src="https://github.com/user-attachments/assets/e01f96bf-f162-4734-9bf1-b83f25383b60" />
Вот ее содержимое
<img width="974" height="527" alt="image" src="https://github.com/user-attachments/assets/6a148342-420e-48cc-8f34-ff05cca784a1" />
Переименованы для удобства. Видна проверка по длине 24. 
Далее производится проверка вида: input == secret ^ key по 3 байта
- secret_data заполняется из функции sub_12C0. Внутри много повторяющегося бесполезного кода – это обфускация. 
Обфускации: 
<img width="622" height="413" alt="image" src="https://github.com/user-attachments/assets/c6241aa6-e505-41f1-9848-5c7bf7d788dc" />
<img width="931" height="1163" alt="image" src="https://github.com/user-attachments/assets/581dcd68-b4bb-42e5-97bb-2edced64df57" />
Важные строки в конце
<img width="741" height="278" alt="image" src="https://github.com/user-attachments/assets/adc4c8b1-084e-4958-9984-b16dcbc60ffc" />
Secret_data состоит из двух частей:
Первая (16 байт) в переменной xmmword_3B40
<img width="974" height="78" alt="image" src="https://github.com/user-attachments/assets/f626d006-c9ca-4d6a-99f1-df06760b681d" />
Вторая (8 байт) добавляется константой 0x14130817005A0E08LL (hex: 0x14130817005A0E08) 
Разворачиваем байты и получаем
Итог: 1d0811130f1749150d0003195a1d1315080e5a0017081314 (24 байта)

var encodedHex = "1d0811130f1749150d0003195a1d1315080e5a0017081314";
var xorKey = "pizzapizzapizzapizzapizzapizza";

var encoded = Convert.FromHexString(encodedHex);
var keyBytes = Encoding.UTF8.GetBytes(xorKey);

var secret = Encoding.UTF8.GetString(
    [.. encoded.Select((b, i) => (byte)(b ^ keyBytes[i % keyBytes.Length]))]
);

Console.WriteLine($"Secret: {secret}");


making owasp great again

Проверка
<img width="588" height="784" alt="image" src="https://github.com/user-attachments/assets/dc5962b6-d941-4283-be8f-4af14709f2ac" />
