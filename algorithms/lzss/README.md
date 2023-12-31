
# Алгоритм Лемпеля-Зива-Сторера-Шимански (LZSS)

## Описание алгоритма


### Кодовое слово

Кодовое слово LZSS состоит из трех частей:

* **Одно-байтовый префикс(Если 0, то не указывается)** - указывает, кодировалась ли последовательность
* **Смещение от начала словаря** - или индекс начала RE(0, если RE не найдено)
* **Длина RE(сам символ, если RE не найдено)** - или кол-во повторяющихся символов

PS: Не найдено RE может быть в том случае, если вторая часть буфера начинается с символа, 
который не встречается в первой части буфера

## Пример

Пусть дано сообщение `синяя_синева_синеет`

Установим:
* Словарь **9** символов
* Размер буфера **6** символов

Тогда составим таблицу для кодирования:

| 1 |  2  |  3  |  4  |  5  |  6  |  7  | 8 |  9  |     |  1  |  2  |  3  |  4  |  5  | 6 |  Код   |
|:-:|:---:|:---:|:---:|:---:|:---:|:---:|:-:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:-:|:------:|
|   |     |     |     |     |     |     |   |     |     | `с` |  и  |  н  |  я  |  я  | _ |  `0с`  |
|   |     |     |     |     |     |     |   |  с  |     | `и` |  н  |  я  |  я  |  _  | с |  `0и`  |
|   |     |     |     |     |     |     | с |  и  |     | `н` |  я  |  я  |  _  |  с  | и |  `0н`  |
|   |     |     |     |     |     |  с  | и |  н  |     | `я` |  я  |  _  |  с  |  и  | н |  `0я`  |
|   |     |     |     |     |  с  |  и  | н | `я` |     | `я` |  _  |  с  |  и  |  н  | е | `181`  |
|   |     |     |     |  с  |  и  |  н  | я |  я  |     | `_` |  с  |  и  |  н  |  е  | в |  `0_`  |
|   |     |     | `с` | `и` | `н` |  я  | я |  _  |     | `с` | `и` | `н` |  е  |  в  | а | `133`  |
| с |  и  |  н  |  я  |  я  |  _  |  с  | и |  н  |     | `е` |  в  |  а  |  _  |  с  | и |  `0e`  |
| и |  н  |  я  |  я  |  _  |  с  |  и  | н |  е  |     | `в` |  а  |  _  |  с  |  и  | н |  `0в`  |
| н |  я  |  я  |  _  |  с  |  и  |  н  | е |  в  |     | `а` |  _  |  с  |  и  |  н  | е |  `0a`  |
| я |  я  | `_` | `с` | `и` | `н` | `е` | в |  а  |     | `_` | `с` | `и` | `н` | `е` | е | `125`  |
| н | `е` |  в  |  а  |  _  |  с  |  и  | н |  е  |     | `е` |  т  |     |     |     |   | `111 ` |
| е |  в  |  а  |  _  |  с  |  и  |  н  | е |  е  |     |  т  |     |     |     |     |   |  `0т`  |



Когда символ ранее никогда не кодировался, вместо длины RE указываем сам символ в коде. 
Также одно-байтовый префикс не указывается, если он равен 0


Декодирование

|  Код   | Печать | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:------:|:------:|---|---|---|---|---|---|---|---|---|
|  `0с`  |   c    |   |   |   |   |   |   |   |   | c |
|  `0и`  |   и    |   |   |   |   |   |   |   | с | и |
|  `0н`  |   н    |   |   |   |   |   |   | с | и | н |
|  `0я`  |   я    |   |   |   |   |   | с | и | н | я |
| `181`  |   я    |   |   |   | с | и | н | я | я | я |
|  `0_`  |   _    |     
| `133`  |   в    |     
|  `0e`  |   а    |     
|  `0в`  | _синее |     
|  `0a`  |   т    |     
| `125`  |        |     
| `111 ` |        |      