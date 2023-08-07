###

but first we have to understand little-endian format and char value:
32 bit little-endian int have the most significant bits on the right
so the value of the in memory hex (0x01, 0x02, 0x03, 0x04) is really (0x04, 0x03, 0x02, 0x01) 
char value 'Y' = 0x59 = 89, 'z' = 0x7a = 122, 'b' = 0x62 = 98

'Y\x00\x00\x00' = 0x59, 0x00, 0x00, 0x00 /Data in memory
'Y\x00\x00\x00' = 0x00000059 = 89 /Real integer value

'z\x03\x00\x00' = 0x7a, 0x03, 0x00, 0x00 /Data in memory
'z\x03\x00\x00' = 0x0000037a = 890 /Real integer value

'b\x0b\x07\x00' = 0x62, 0x07, 0x00, 0x00 /Data in memory
'b\x0b\x07\x00' = 0x00000762 = 1890 /Real integer value

### convert from a number to simple byte and 32 bytes
```python
for digit in range(1, 50):
    print(digit)
    digit_byte_big = digit.to_bytes(32, 'big')
    print('simple byte {} = big byte = {}'.format(digit.to_bytes(), digit_byte_big))
```
[result see the file](./files/get32byteDigit.txt)

### Сколько бит занимает одно шестнадцатеричное число

Одна шестнадцатеричная цифра занимает четыре бита (четыре бита часто называют полубайтом или тетрадой). Двузначное шестнадцатеричное число занимает восемь бит (один байт). Это может быть, например, регистр BL. При выводе числа на экран сначала выводится старшая цифра, а затем – младшая.

### Управляющие символы
 https://translated.turbopages.org/proxy_u/en-ru.ru.1a991070-64ac71bd-313995d5-74722d776562/https/en.wikipedia.org/wiki/Control_character