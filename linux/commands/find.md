$ find . -iname "s.txt"
$ find . -iname \*.png
$ find /home -name "\*.png"

f – простые файлы;
d – каталоги;
l – символические ссылки;
b – блочные устройства (dev);
c – символьные устройства (dev);
p – именованные каналы;
s – сокеты;

$ find . -type d
.
./.ssh
./.cache
./moodle

### find by size

$ find . -size +1G
"+" — Поиск файлов больше заданного размера
"-" — Поиск файлов меньше заданного размера
c — Байт
k — Кбайт
M — Мбайт
G — Гбайт

(by time) $ find . -cmin -60
(by user) $ find /home -user tisha 2>/dev/null
(by permission) $ find /home -perm 777
$ find /home -user tisha -and -size +1G 2>/dev/null

К команде find можно добавить действия, которые будут произведены с результатами поиска.

-delete — Удаляет соответствующие результатам поиска файлы
-ls — Вывод более подробных результатов поиска с:
Размерами файлов.
Количеством inode.
-print Стоит по умолчанию, если не указать другое действие. Показывает полный путь к найденным файлам.
-exec Выполняет указанную команду в каждой строке результатов поиска.

(delete all empty files) $ find . -empty -delete

() > -exec command {} \;

Где:

command – это команда, которую вы желаете выполнить для результатов поиска. Например:
rm
mv
cp
{} – является результатами поиска.
\; — Команда заканчивается точкой с запятой после обратного слеша.

(alternative for delete) $ find . -empty -exec rm {} \

### comments

Еще хороший инструмент: fdupes -rd .
Ну и конечно: du -Sh | sort -rh | head -20
А так же
find. -type f -exec sudo chmod 664 {} \;
find. -type d -exec sudo chmod 775 {} \;
