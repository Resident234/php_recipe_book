# `Строки`

## 1.1. Доступ к подстрокам
### Задача
Предположим, что требуется выделить часть строки, начиная с опре
деленной позиции. Например, необходимы первые восемь символов
имени пользователя, введенного в форму.

### Решение
```php
$substring = substr($string, $start, $length);
$username = substr($_REQUEST['username'], 0, 8);
```

### Обсуждение
Если `$start` и `$length` больше нуля, то функция `substr()` возвращает
`$length` символов строки, начиная с позиции `$start`. Первый символ находится в нулевой позиции:
```php
print substr('watch out for that tree', 6, 5);
out f
```
Если пропустить `$length`, то функция `substr()` возвратит строку, начиная с позиции `$start` до конца первоначальной строки:
```php
print substr('watch out for that tree', 17);
t tree
```
Если `$start` плюс `$length` указывает на позицию за концом, то функция
`substr()` возвращает всю строку до конца, начиная с позиции `$start`:
```php
print substr('watch out for that tree', 20, 5);
ree
```
Если число `$start` отрицательное, то функция `substr()` определяет начальную позицию подстроки, считая от конца строки к началу:
```php
print substr('watch out for that tree',	-6);
print substr('watch out for that tree',	-17, 5);
t tree
out f
```
Если число `$length` отрицательное, то `substr()` определяет конечную
позицию подстроки, считая от конца строки к началу:
```php
print substr('watch out for that tree', 15, -2);
print substr('watch out for that tree',	-4,	-1);
hat t
tre
```

## 1.2. Замещение подстрок
### Задача
Требуется заменить подстроку другой строкой. Например, перед тем
как напечатать номер кредитной карты, вы хотите скрыть все цифры
ее номера, за исключением последних четырех.

### Решение
```php
// Все, начиная с позиции $start до конца строки $old_string,
// заносится в $new_substring
$new_string = substr_replace($old_string, $new_substring, $start);
// $length символов, начиная с позиции $start, заменяются на $new_substring
$new_string = substr_replace($old_string, $new_substring, $start, $length);
```

### Обсуждение
Без аргумента `$length`, функция `substr_replace()` заменяет все, начиная
с позиции `$start` до конца строки. Если значение `$length` определено,
то замещается только это количество:
```php
print substr_replace('My pet is a blue dog.', 'fish.', 12);
print substr_replace('My pet is a blue dog.', 'green', 12, 4);
$credit_card = '4111 1111 1111 1111';
print substr_replace($credit_card, 'xxxx', 0, strlen($credit_card)-4);
```
```
My pet is a fish.
My pet is a green dog.
xxxx 1111
```
Если число `$start` отрицательное, то новая подстрока помещается в позицию `$start`, считая от конца строки `$old_string`, а не с начала:
```php
print substr_replace('My pet is a blue dog.','fish.', -9);
print substr_replace('My pet is a blue dog.','green', -9,4);
```
```
My pet is a fish.
My pet is a green dog.
```
Если `$start` и `$length` равны 0, то новая подстрока вставляется в начало
`$old_string`:
```php
print substr_replace('My pet is a blue dog.', 'Title: ', 0, 0);
```
```
Title: My pet is a blue dog.
```
Функция `substr_replace()` удобна, когда текст невозможно отобразить
за один раз, и вы хотите показать его часть со ссылкой на остальное содержание. Например, следующее выражение отображает 25 строк текста с многоточием в качестве ссылки на страницу с продолжением:
```php
$r = mysql_query("SELECT id, message FROM messages WHERE id = $id") or die();
$ob = mysql_fetch_object($r);
printf('<a href="more-text.php?id=%d">%s</a>', $ob->id, substr_replace($ob->message, ' ...', 25));
```
На странице `more-text.php` для извлечения полного текста сообщения
и его отображения может использоваться идентификатор сообщения,
переданный в строке запроса.

## 1.3. Посимвольная обработка строк
### Задача
Нужно обработать каждый символ строки по отдельности.

### Решение
Цикл по символам строки с помощью оператора for. В этом примере
подсчитываются гласные в строке:
```php
$string = "This weekend, I'm going shopping for a pet chicken.";
$vowels = 0;
for ($i = 0, $j = strlen($string); $i < $j; $i++) {
    if (strstr('aeiouAEIOU',$string[$i])) {
        $vowels++;
    }
}
```

### Обсуждение

## 1.4. Пословный или посимвольный переворот строки
### Задача
Требуется перевернуть слова или символы в строке.

### Решение
Для посимвольного переворота строки применяется функция `strrev()`:
```php
print strrev('This is not a palindrome.');
```
```
.emordnilap a ton si sihT
```
Чтобы перевернуть строку пословно, надо разобрать строку на слова,
перевернуть слова, а затем собрать их заново в строку:
```php
$s = "Once upon a time there was a turtle.";
// разбиваем строку на слова
$words = explode(' ', $s);
// обращаем массив слов
$words = array_reverse($words);
$s = join(' ', $words);
print $s;
```
```
turtle. a was there time a upon Once
```

### Обсуждение

## 1.5. Расширение и сжатие табуляций
### Задача
Нужно заменить пробелы на табуляцию (или табуляцию на пробелы)
и в то же время сохранить выравнивание теста по позициям табуляции. Например, вы хотите отобразить для пользователя текст стандартным образом.

### Решение
Для замены пробелов на табуляцию или табуляции на пробелы следует применять функцию `str_replace()`:
```php
$r = mysql_query("SELECT message FROM messages WHERE id = 1") or die();
$ob = mysql_fetch_object($r);
$tabbed = str_replace(' ', "\t", $ob->message);
$spaced = str_replace("\t", ' ', $ob->message);
print "With Tabs: <pre>$tabbed</pre>";
print "With Spaces: <pre>$spaced</pre>";
```
Однако если для преобразования применяется функция `str_replace()`,
то позиции табуляции нарушаются. Если вы хотите ставить табуляцию через каждые восемь символов, то в строке, начинающейся с пятибуквенного слова и табуляции, необходимо заменить табуляцию на
три пробела, а не на один. Для замены табуляции на пробелы с учетом
позиций табуляции следует применять функцию `pc_tab_expand()`, показанную в примере 1.1.
```
Пример 1.1. pc_tab_expand()
```
```php
function pc_tab_expand($a) {
    $tab_stop = 8;
    while (strstr($a, "\t")) {
        $a = preg_replace('/^([^\t]*)(\t+)/e', "'\\1' . str_repeat(' ', strlen('\\2') * $tab_stop - strlen('\\1') % $tab_stop)", $a);
    }
    return $a;
}
$spaced = pc_tab_expand($ob->message);
```

Для обратной замены пробелов на табуляцию можно воспользоваться
функцией `pc_tab_unexpand()`, показанной в примере 1.2.
```
Пример 1.2. pc_tab_unexpand()
```
```php
function pc_tab_unexpand($x) {
    $tab_stop = 8;

    $lines = explode("\n", $x);
    for ($i = 0, $j = count($lines); $i < $j; $i++) {
        $lines[$i] = pc_tab_expand($lines[$i]);
        $e = preg_split("/(.\{$tab_stop})/", $lines[$i], 1, PREG_SPLIT_DELIM_CAPTURE);
        $lastbit = array_pop($e);
        if (!isset($lastbit)) { $lastbit = ''; }
        if ($lastbit == str_repeat(' ', $tab_stop)) { $lastbit = "\t"; }
        for ($m = 0, $n = count($e); $m < $n; $m++) {
            $e[$m] = preg_replace('/ +$', "\t", $e[$m]);
        }
        $lines[$i] = join('', $e) . $lastbit;
    }
    $x = join("\n", $lines);
    return $x;
}
$tabbed = pc_tab_unexpand($ob->message);
```
Обе функции принимают в качестве аргумента строку и возвращают
ее, модифицировав соответствующим образом.
 
### Обсуждение

## 1.6. Управление регистром
### Задача
Необходимо переключиться на прописные буквы, или в нижний регистр или иным образом изменить регистр символов в строке. Например, требуется сделать прописными начальные буквы имен и сохранить нижний регистр остальных букв.

### Решение
Первые буквы одного или более слов можно сделать прописными с помощью функции `ucfirst()` или функции `ucwords()`:
```php
print ucfirst("how do you do today?");
print ucwords("the prince of wales");
```
```
How do you do today?
The Prince Of Wales
```
Регистр всей строки изменяется функцией `strtolower()` или функцией
`strtoupper()`:
```php
print strtoupper("i'm not yelling!");
print strtolower('<A HREF="one.php">one</A>');
```
```
I'M NOT YELLING!
<a href="one.php">one</a>
```

### Обсуждение

## 1.7. Включение функций и выражений в строки
### Задача
Вставить результаты выполнения функции или выражения в строку.

### Решение
Когда значение, которое необходимо вставить в строку, не может быть
в нее включено, следует применять оператор конкатенации строк (.):
```php
print 'You have ' . ($_REQUEST['boys'] + $_REQUEST['girls']) . ' children.';
print "The word '$word' is " . strlen($word) . ' characters long.';
print 'You owe ' . $amounts['payment'] . ' immediately';
print "My circle's diameter is " . $circle->getDiameter() . ' inches.';
```

### Обсуждение

## 1.8. Удаление пробельных символов из строки
### Задача
Надо удалить пробельные символы в начале или в конце строки. Например, привести в порядок данные, введенные пользователем, прежде чем счесть их действительными.

### Решение
Следует обратиться к функциям `ltrim()`, `rtrim()` или `trim()`. Функция
`ltrim()` удаляет пробельные символы в начале строки, `rtrim()` – в конце
строки, а функция `trim()` – и в начале, и в конце строки:
```php
$zipcode = trim($_REQUEST['zipcode']);
$no_linefeed = rtrim($_REQUEST['text']);
$name = ltrim($_REQUEST['name']);
```

### Обсуждение

## 1.9. Анализ данных, разделенных запятой
### Задача
Есть данные, разделенные запятыми (формат CSV), например, файл,
экспортированный из Excel или из базы данных, и необходимо извлечь записи и поля в формате, с которым можно работать в PHP.

### Решение
Если CSV данные представляют собой файл (или они доступны через
URL), то откройте файл с помощью функции `fopen()` и прочитайте данные с помощью функции `fgetcsv()`. Данные будут представлены в виде
HTML-таблицы:
```php
$fp = fopen('sample2.csv', 'r') or die("can't open file");
print "<table>\n";
while ($csv_line = fgetcsv($fp, 1024)) {
    print '<tr>';
    for ($i = 0, $j = count($csv_line); $i < $j; $i++) {
        print '<td>' . $csv_line[$i] . '</td>';
    }
    print "</tr>\n";
}
print '</table>\n';
fclose($fp) or die("can't close file");
```

### Обсуждение

## 1.10. Анализ данных, состоящих из полей фиксированной ширины
### Задача
Необходимо разбить на части записи фиксированной ширины в строке.

### Решение
Это делается при помощи функции `substr()`:
```php
$fp = fopen('fixed-width-records.txt','r') or die ("can't open file");
while ($s = fgets($fp,1024)) {
    $fields[1] = substr($s,0,10); // первое поле: первые 10 символов строки
    $fields[2] = substr($s,10,5); // второе поле: следующие 5 символов строки
    $fields[3] = substr($s,15,12); // третье поле: следующие 12 символов строки
    // функция обработки полей
    process_fields($fields);
}
fclose($fp) or die("can't close file");
```
Или функции `unpack()`:
```php
$fp = fopen('fixed-width-records.txt','r') or die ("can't open file");
while ($s = fgets($fp,1024)) {
    // ассоциативный массив с ключами "title", "author" и "publication_year"
    $fields = unpack('A25title/A14author/A4publication_year',$s);
    // функция обработки полей
    process_fields($fields);
}
fclose($fp) or die("can't close file");
```

### Обсуждение

## 1.11. Разбиение строк
### Задача
Необходимо разделить строку на части. Например, нужно получить
доступ к каждой из строк, которые пользователь вводит в поле `<textarea>` формы.

### Решение
Если в качестве разделителя частей строк выступает строковая константа, то следует применять функцию `explode()`:
```php
$words = explode(' ','My sentence is not very complicated');
```
Функция `split()` или функция `preg_split()` применяются, если при описании разделителя требуется регулярное выражение POSIX или Perl:
```php
$words = split(' +', 'This sentence has some extra whitespace in it.');
$words = preg_split('/\d\. /', 'my day: 1. get up 2. get dressed 3. eat toast');
$lines = preg_split('/[\n\r]+/', $_REQUEST['textarea']);
```
В случае чувствительного к регистру разделителя применяется функция `spliti()` или флаг `/i` в функции `preg_split()`:
```php
$words = spliti(' x ', '31 inches x 22 inches X 9 inches');
$words = preg_split('/ x /i', '31 inches x 22 inches X 9 inches');
```

### Обсуждение

## 1.12. Упаковка текста в строки определенной длины
### Задача
Необходимо упаковать линии текста в строку. Например, нужно отобразить текст, содержащийся в тегах `<pre>/</pre>`, в пределах окна браузера обычного размера.

### Решение
Это делается при помощи функции `wordwrap()`:
```php
$s = "Four score and seven years ago our fathers brought forth on this
continent a new nation, conceived in liberty and dedicated to the proposition
that all men are created equal.";
print "<pre>\n".wordwrap($s)."\n</pre>";
```
```
<pre>
Four score and seven years ago our fathers brought forth on this continent
a new nation, conceived in liberty and dedicated to the proposition that
all men are created equal.
</pre>
```

### Обсуждение

## 1.13. Хранение двоичных данных в строках
### Задача
Необходимо проанализировать строку, которая содержит значения,
закодированные с помощью двоичной структуры, или закодировать
значения в строку. Например, нужно сохранить числа в их двоичном
представлении, а не как последовательность ASCII-символов.

### Решение
Для сохранения двоичных данных в строке применяется функция `pack()`:
```php
$packed = pack('S4',1974,106,28225,32725);
```
Функция `unpack()` позволяет извлекать двоичные данные из строки:
```php
$nums = unpack('S4',$packed);
```

### Обсуждение

