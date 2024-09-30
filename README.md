# 🦀 Язык программирования Rust

## Содержание:
- [Установка](#установка)
- [Cargo](#cargo)
    - [Cargo.toml](#cargotoml)
- [Общие концепции программирования](#общие-концепции-программирования)
    - [Переменные](#переменные)
    - [Типы данных](#типы-данных)
    - [Выражения и инструкции](#выражения-и-инструкции)
    - [Функции](#функции)
    - [Break и continue](#break-и-continue)
- [Список используемой литературы](#список-используемой-литературы)
- [Необходимо добавить](#необходимо-добавить)


## Установка
> [!NOTE]
> Данные инструкции применимы для Windows. Если у вас другая ОС выполните другие [действия](https://doc.rust-lang.ru/book/ch01-01-installation.html#installation)

- Установить [Rust](https://www.rust-lang.org/tools/install)  
Проверку можно осуществить следующей командой: `rustc --version` в PowersShell

- Установить VS и скачать пакет для работы с С++

## Cargo
Компилировать Rust файл можно командой `rustc example.rs`. И это удобно, когда вся программа хранится в одном файле, но если их будет много, то лучше использовать Cargo. 

Cargo - это система сборки и менеджер пакетов Rust (аналог Makefile). Он скачивается при установке Rust. Проверить его наличие можно командой `cargo --version`.

Есть несколько основных команд для работы с Cargo:
- `cargo new example` - создание каталога example, в котором хранится [Cargo.toml](#cargotoml) и папка src c файлом main.rs внутри.
- `cargo build` - сборка программы (прописывать команду нужно находять в каталоге example). Исполняемый файл будет сохраняться в target\debug\hello_cargo.exe, а не в текущем каталоге.
- `cargo run` - сборка и запуск программы 
- `cargo check` - проверка программы на компиляцию без создания исполняемого файла
- `cargo build --release` - компиляция программы с оптимизацией для финального результата. Исполняемый файл помещается в папку target\release.

> [!TIP]
> Результат выполнения некоторых команд (1, 2, 5) можно посмотреть в папке les1

### Cargo.toml
Только сгенерированный файл будет выглядеть примерно так:  
```powershell
[package]
name = "example"
version = "0.1.0"
edition = "2021"

[dependencies]
```

[package], является заголовочной секцией, которая указывает что следующие инструкции настраивают пакет. По мере добавления информации в данный файл, будет добавляться больше секций и инструкций (строк).

Следующие три строки задают информацию о конфигурации, необходимую Cargo для компиляции программы: имя, версию и редакцию Rust, который будет использоваться (в более новых версиях могут добавляться некоторые ключевые слова, для того, чтобы они работали и нужно указывать необходимую версию)

Последняя строка, [dependencies] является началом секции для списка любых зависимостей проекта

## Общие концепции программирования
> [!TIP]
> Практическое применение рассматриваемого материала из этой главы можно посмотреть в папке les2

### Переменные

В Rust есть несколько видов переменных:  
- Неизменяемые переменные: `let x = 5;`  
- Изменяемые переменные: `let mul x = 5;`
- Константы: `const PI = 3.14;`

Константы от неизменяемых переменных отличаются тем, что для констант необходимо сразу же указывать тип данных и значение. А в неизменяемую переменную можно изначально просто объявить, а потом уже присвоить ей значение.

### Типы данных

Есть два подмножества типов данных: скалярные и составные.  
Начнем со скалярных:
- Целочисленный тип  
Для обозначения целочисленного типа пишется буква `i` и количество бит, которое будет занимать переменная (8, 16, 32, 64, 128, size). Например: `i32`.  
Если в переменной будут содержаться только положительные числа, то используется буква `u`. Например: `u32`.  
Количество занимаемой памяти при size зависит от архитектуры компьбтера (32 или 64 бит).  
По умолчанию используется `i32`.
- Числа с плавающей точкой  
Есть всего два вида: `f32` и `f64`. Оба являются знаковыми.  
По умолчанию испльзуется `f64`.
- Логический тип  
Обозначается как `bool`. Имеет два значения: `true` или `false`. Занимает 1 байт.
- Символьный тип  
Обозначается как `char`. Тип `char` в Rust имеет размер четыре байта и представляет собой скалярное значение Unicode, так что в переменную можно записывать не только буквы  цифры, но и иероглифы, смайлики и тд.

Перейдем к составным типам данных:
- Массивы  
Суть массивов такая же как в Си, там хранится определенное количество значений одного типа данных.  
Можно сразу проинициализировать массив: `let mas = [1,2,3,4];`  
Или же сначала выделить память: `let mas: [i8,5];`  
В Rust также можно заполнить весь массив определенным значением: `let mas: [0,3];`. Это будет аналогично: `let mas = [0,0,0];`
Получение значения делается также как в Си через []. Индексация начинается с 0.  
Если произойдет ситуация, что во время выполнения программы мы попытаемся обратиться к индексу, выходящему за пределы массива, то программа завершит свою работу.
- Кортежи  
Кортежи напоминают массивы, только там могут хранится разные типы данных (аналог структур).  
Мы так же можем сразу инициализировать переменную: `let tup: (i32,f32,u8) = (500,5.32,3);`. Указание типов можно опустить, просто подставятся те, что идут по умолчанию.  
Или сначала выделить память: `let tup: (i32,f32,u8);`  
Получить же значение мы можем двумя способами:  
  1. Присвоить ряду переменных значения кортежа, и потом просто обращаться к этим переменным:
    ``` Rust
    let tup: (i32, f32, u8) = (500,5.32,2);
    let (x,y,z) = tup;
    println!("y={}",y); //y=5.32
    ```
  2. Обращаться к элементам через точку:
    ``` Rust
    let tup: (i32, f32, u8) = (500,5.32,2);
    println!("tup.1={}",tup.1); //tup.1=5.32
    ```

### Выражения и инструкции

Инструкции выполняют некое действие и не возвращают никакого значения.  
Например: 
``` Rust
fn main() {
    let a = 5;
}
```

Выражения же вычисляются до определенного результата, которое и будет возвращено.
Например:
``` Rust
fn main() {
    let a = {
        let b = 5;
        b + 1
    }; //a = 6
}
```
После выражений не ставится `;` !

### Функции

Работа с функциями в Rust практически не отличается от Си:
- `fn title_function() {}` - объявление функции (объявлять функцию можно и после main, к ней все равно будет доступ)
- `title _function()` - вызов функции
- `fn title_function(a: i32, b: char) {}` - объявление функции с параметрами. Указание типов переменных обязательно.  
Если параметр будет изменяться в функции, то необходимо добавить `mut` перед названием переменной
- `fn title_fun() -> u16 {}` - объявление функции, которая возвращает некое значение.

### Управляющие конструкции

- if else  
    В целом работа `if` такая же как в Си:
    ``` Rust
    let a = 10;
    if a==10 {
    println!("a=10");
    } else if a<10 {
        println!("a<10");
    } else {
        println!("a>10");
    }
    ```
    Единственное, условие всегда должно быть условного типа. Если передать просто число, то компилятор выдаст ошибку.  
    А так как `if` является в Rust выражением, то с помощью него можно выполнять тернарную операцию:
    ``` Rust
    let f = true;
    let a = if f {1} else {2};
    ```

- loop  
    `loop` - это цикл, который будет выполняться бесконечно, пока не нажать Ctrl+C или не написать явно условие для выхода используя [break](#break-и-continue).

- while  
    `while` - цикл, который выполняется пока условие истинно. Аналогичен Си.

- for
    `for` используется для переборов коллекций. Например:
    ``` Rust
    let mas = [1,2,3,4,5];
    for elem in mas {
        println!("{elem}");
    }
    ```

### break и continue

`break` - выход из цикла. Если `break` находится в блоке, где есть неколько вложенных циклов, тогда он применяется только к внутреннему.

`continue` - заканчивается выполнение текущей итерации и начинается выполнение следующей.

Чтобы применять `break` и `continue` к внешним циклам, можно на циклы установить специальные метки с помощью `'`:
``` Rust
let mut count = 1;
'metka: loop {
    count *= 2;
    loop {
        count += 1;
        if count % 2 == 0 {
            break;
        } 
        if count > 20 {
            break 'metka;
        }
    }
}
```

Так же с помощью `break` можно возвращать некие значения:
``` Rust
let mut count = 0;
loop {
    count +=1;
    if count == 10 {
        break count * 2;
    }
}
```

## Список используемой литературы
- [Документация](https://doc.rust-lang.ru/book/title-page.html)
- [Документация Cargo](https://doc.rust-lang.org/cargo/index.html)
- [Блоки уведомлений в Markdown](https://www.freecodecamp.org/news/how-to-create-notice-blocks-in-markdown/)

## Необходимо добавить
- [ ] Инструкции в Cargo.toml
- [ ] Добавление зависимостей [dependencies]
- [ ] Различия Cargo.toml и Cargo.lock