# Явное аннотирование

Анализатор заимствований использует явные аннотации времён 
жизни для определения того, как долго ссылки будут 
действительны. В случаях, когда времена жизни не скрыты[^1], Rust 
требует их явного аннотирования, чтобы определить какое у 
ссылки должно быть время жизни. Для явного аннотирования 
времени жизни используется синтаксис с символом апострофа, как 
тут:

```rust,ignore
foo<'a>
// `foo` имеет параметр времени жизни `'a`
```

Подобно [замыканиям](../../fn/closures/anonymity.md), явное использование времён жизни 
требует обобщённого параметра. Кроме того, такой синтаксис 
показывает, что время жизни `foo` не может 
превышать `'a`. Явная аннотация для типа имеет 
форму `&'a T`, где `'a` уже задана.

В случаях со множественными временами жизни, синтаксис будет 
подобен следующему:

```rust,ignore
foo<'a, 'b>
// `foo` имеет параметры времён жизни `'a` и `'b`
```

В данном случае, время жизни `foo` не может 
превышать *ни `'a`, ни `'b`*.

Рассмотрим следующий пример, в котором используется явная аннотация времён жизни:

```rust,editable,ignore,mdbook-runnable
// `print_refs` получает две ссылки на `i32`, имеющие различные
// времена жизни `'a` и `'b`. Оба этих времени жизни должны существовать
// не меньше, чем функция `print_refs`.
fn print_refs<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("x равно {} и y равно {}", x, y);
}

// Функция, не имеющая аргументов, но имеющая параметр времени жизни `'a`.
fn failed_borrow<'a>() {
    let _x = 12;

    // ОШИБКА: `_x` не живёт достаточно долго (`_x` does not live long enough)
    //let y: &'a i32 = &_x;
    // Попытка использования времени жизни `'a` для явного аннотирования 
    // внутри функции приведёт к ошибке, так как время жизни у `&_x` короче, чем
    // у `y`. Коротное время жизни не может быть приведено к длинному.
}

fn main() {
    // Создадим переменные, которые далее будут заимствованы.
    let (four, nine) = (4, 9);
    
    // Заимствуем (`&`) обе переменные и передадим их в функцию.
    print_refs(&four, &nine);
    // Любой ввод, который заимствуется, должен жить дольше, чем заимствующий. 
    // Другими словами, время жизни `four` и `nine` должно
    // быть больше, чем время жизни `print_refs`.
    
    failed_borrow();
    // `failed_borrow` не содержит ссылок, заставляющих `'a` быть
    // больше, чем время жизни функции, но `'a` больше.
    // Поскольку время жизни никогда не ограничено, оно, по умолчанию, равно `'static`.
}
```

[^1]: [сокрытие](elision.md) позволяет скрыть аннотации времён жизни, но они всё же присутствуют.

### Смотрите также:

[Обобщения](../../generics.md) и [замыкания](../../fn/closures.md)