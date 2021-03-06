# macro_rules!

Rust предоставляет мощную систему макросов, которая позволяет
использовать метапрограммирование. Как вы могли видеть в предыдущих главах,
макросы выглядят как функции, но их имя заканчивается восклицательным знаком (`!`).
Вместо вызова функции, макросы расширяются в исходный код, который впоследствии
компилируется с остальной частью программы.
Однако, в отличие от макросов на C и других языках, макросы Rust расширяются
в абстрактные синтаксические деревья, а не в подстановку строк,
поэтому Вы не получаете неожиданных ошибок приоритета операций.

Макросы создаются с помощью макроса `macro_rules!`

```rust,editable
// Этот простой макрос называется `say_hello`.
macro_rules! say_hello {
    // `()` указывает, что макрос не принимает аргументов.
    () => (
        // Макрос будет раскрываться с содержимым этого блока.
        println!("Hello!");
    )
}

fn main() {
    // Этот вызов будет раскрыт в код `println!("Hello");`
    say_hello!()
}
```

Так почему же макросы полезны?

1. Не повторяйтесь. Есть много случаев, когда вам может понадобиться подобная
    функциональность в нескольких местах, но с различными типами. Чаще всего написание
    макроса - это полезный способ избежать повторения кода. (Подробнее об этом позже)

2. Предметно-ориентированные языки. Макросы позволяют определить специальный синтаксис для
    конкретной цели. (Подробнее об этом позже)

3. Вариативные интерфейсы. Иногда требуется определить интерфейс, который
    имеет переменное количество аргументов. Пример: `println!`, который может принять любое
    количество аргументов в зависимости от строки формата. (Подробнее об этом позже)
