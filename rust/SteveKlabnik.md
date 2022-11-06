Установка Linux & MacOS
С помощью этой команды скачиваем скрипт, запускающий установку иструмента rustup,
который устанавливает последнюю стабильную версию языка Rust.
Перезапустите командную оболочку.

>curl https://sh.rustup.rs -sSf | sh

Обновление и деинсталяция Rust:
> rustup update\
> rustup self uninstall

> rustup --version

### impl
- реалзиуем методы
- реализуем типаж в типе: 'impl Trait for Type {}'
- передаем типажи в качестве параметров функции: 'pub fn notify(item: impl Trait) {}' Этот параметр принимает любой
тип, реализующий указанный типаж.
- предыдущий пункт - синтаксический сахар от 'pub fn notify<T: Trait>(item: T) {}'. В этом коде больше слов.
Если функции передаются два одинаковых аргумента, то лучше использовать:'pub fn notify<T: Trait>(item1: T, item2: T) {}'
- Определяем notify что item должен реализовать как Display, так и Summary: 'pub fn notify(item: impl Summary + Display) {...}'
- Или то же самое, только через общий тип: 'pub fn notify<T: Summary + Display>(item: T) {...}'
- Более четкие границы  типажа с условием 'where':
```rust
fn some_function<T: Display + Clone, U: Clone + Debug>(t: T, u: U) -> i32 {}

fn some_function<T, U>(t: T, u: U) -> i32
	where T: Display + Clone,
		U: Clone + Debug
{}
```
- Возвращаем 'fn returns_summarizable() -> impl Summary {...}'
- !Синтаксис 'impl Trait' позволяет кратко описать, что функция возвращает некий тип, реазлизующий типаж, без необходимости расписывать очень длинный тип
- Однако воспользоваться 'impl Trait' можно тлько если возвращаем один тип: например, получим ошибку если будем использовать 'if () { Tweet {...}} else { NewArticle {...}}'
-- 