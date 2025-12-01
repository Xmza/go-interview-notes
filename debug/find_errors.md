# 🐞 Найди ошибку / Code Review

В этом разделе собраны популярные ошибки, баги и "ловушки" языка Go.
**Задача:** Посмотри на код, попытайся понять, что с ним не так (или что он выведет), и только потом открой спойлер.

---

## 1. Loop Variable Capture (Классика до Go 1.22)

Что выведет этот код? (Предполагаем версию Go < 1.22 или рассматриваем классическую проблему замыкания).

```go
package main
import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 5; i++ {
        wg.Add(1)
        go func() {
            fmt.Print(i) // Что тут напечатается?
            wg.Done()
        }()
    }
    wg.Wait()
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

Результат:

Скорее всего, выведется 55555 (или набор пятерок).

Почему:

В старых версиях Go переменная i одна на весь цикл. Горутины запускаются не моментально. К моменту, когда горутина начнет выполнение, цикл уже может завершиться, и i будет равна 5.

Как исправить:

Передавать переменную в аргументы анонимной функции или создавать локальную копию:

```Go
go func(val int) {
    fmt.Print(val)
    wg.Done()
}(i)
// ИЛИ
i := i
go func() { fmt.Print(i) ... }()
```

> Примечание: В Go 1.22+ это поведение изменили, и там будет выведено 01234 (в случайном порядке).
</details>


## 2. Чтение из закрытого канала
Что произойдет при выполнении этого кода?

```Go
package main
import "fmt"

func main() {
    ch := make(chan int, 1)
    ch <- 1
    close(ch)
    
    val, ok := <-ch
    fmt.Printf("%d %v\n", val, ok)
    
    val2, ok2 := <-ch
    fmt.Printf("%d %v\n", val2, ok2)
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

  Результат:

```Code
1 true
0 false
```
Почему:

Первое чтение забирает значение 1, которое было в буфере. Канал закрыт, но буфер читается.

Второе чтение происходит из пустого закрытого канала. Паники не будет!

При чтении из закрытого канала возвращается "нулевое значение" для типа канала (для int это 0) и флаг false, означающий, что канал закрыт и пуст.
</details>

## 3. Запись в nil map
Что не так с этим кодом?

```Go
package main

func main() {
    var m map[string]int
    m["one"] = 1
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

  Ошибка: Panic (assignment to entry in nil map)

Почему:

Объявление var m map[string]int создает nil map. Она не инициализирована. Читать из неё можно (вернет 0), но писать в неё нельзя — это вызывает панику.

Как исправить:

Нужно инициализировать мапу перед записью:

```Go
m := make(map[string]int)
// ИЛИ
m := map[string]int{}
```
</details>


## 4. Race Condition с WaitGroup
Где здесь спряталась гонка данных (Race Condition)?

```Go
package main
import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 5; i++ {
        go func() {
            wg.Add(1) // <-- Внимательно тут
            fmt.Println("Work")
            wg.Done()
        }()
    }
    wg.Wait()
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

  Ошибка: Race Condition / Негативный счетчик WG

Почему:

Вызов wg.Add(1) находится внутри горутины.

Планировщик может не успеть запустить горутину до того, как main дойдет до wg.Wait().

Если wg.Wait() сработает раньше, чем хотя бы один wg.Add(1), счетчик будет 0, программа завершится, и горутины не выполнятся. Или возникнет паника, если Add сработает во время Wait.

Как исправить:

wg.Add всегда должен вызываться ПЕРЕД запуском горутины.
  
```Go
wg.Add(1)
go func() { ... }()
```
</details>

## 5. Append и перевыделение памяти
Чему будет равен s1 в конце?

```Go
package main
import "fmt"

func main() {
    s1 := []int{1, 2, 3}
    s2 := append(s1, 4)
    s2[0] = 100
    fmt.Println(s1)
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

  Результат: [1 2 3] (не изменился)

Почему:

Изначально у s1 длина 3 и, скорее всего, ёмкость 3.

Когда мы делаем append(s1, 4), ёмкости не хватает. Go создает новый массив большей емкости, копирует туда данные и добавляет 4.

s2 теперь указывает на новый массив. Изменение s2[0] = 100 меняет новый массив, а старый (на который смотрит s1) остается нетронутым.

> Нюанс: Если бы s1 был создан как make([]int, 3, 10), то перевыделения бы не было, и s1 тоже изменился бы.
</details>


## 6. Defer и порядок выполнения
Что выведет этот код?

```Go
package main
import "fmt"

func main() {
    defer fmt.Print("A")
    defer fmt.Print("B")
    fmt.Print("C")
    panic("Panic!")
    defer fmt.Print("D")
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

  Результат: CB A + Panic text

Почему:
- fmt.Print("C") выполняется сразу -> C.
- defer работают по принципу LIFO (стек: последний вошел — первый вышел).
- defer fmt.Print("D") не был зарегистрирован, так как паника случилась до этой строки. Он не выполнится.
- Срабатывает паника, начинается раскрутка стека.
- Выполняется defer fmt.Print("B") -> B.
- Выполняется defer fmt.Print("A") -> A.
- Программа падает с ошибкой.
</details>


## 7. Nil Interface Trap
Будет ли напечатано "Is nil"?

```Go
package main
import "fmt"

func Foo() error {
    var err *MyError = nil
    return err
}

type MyError struct{}
func (e *MyError) Error() string { return "oops" }

func main() {
    err := Foo()
    if err == nil {
        fmt.Println("Is nil")
    } else {
        fmt.Println("Not nil")
    }
}
```
<details>
<summary><b>🔍 Показать ответ</b></summary>

  Результат: Not nil

Почему:

Это знаменитая ловушка интерфейсов.

Интерфейс в Go — это пара (Type, Value).

В функции Foo мы возвращаем nil указатель типа *MyError.

В main переменная err (типа error, то есть интерфейс) содержит:

- Type: *MyError
- Value: nil

Интерфейс равен nil только тогда, когда И тип, И значение равны nil. Здесь тип есть, поэтому err != nil.
</details>


## 8. Изменение элемента в range

Изменится ли возраст пользователей?

```go
package main
import "fmt"

type User struct { Name string; Age int }

func main() {
    users := []User{{"Bob", 20}, {"Alice", 30}}
    for _, u := range users {
        u.Age++
    }
    fmt.Println(users)
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат: `[{Bob 20} {Alice 30}]` (не изменились)

### Почему:
В цикле `range` переменная `u` — это **копия** элемента слайса.
`u.Age++` изменяет поле в копии. Оригинальный элемент в слайсе `users` остается нетронутым.

### Как исправить:
Использовать индекс для обращения к оригиналу:
```go
for i := range users {
    users[i].Age++
}
```

</details>

---

## 9. JSON и приватные поля

Почему JSON пустой?

```go
package main
import (
    "encoding/json"
    "fmt"
)

type User struct {
    name string
    age  int
}

func main() {
    u := User{name: "Bob", age: 25}
    b, _ := json.Marshal(u)
    fmt.Println(string(b))
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат: `{}` (пустой объект)

### Почему:
Поля `name` и `age` написаны с **маленькой буквы**. В Go это означает, что они **неэкспортируемые** (private).
Пакет `encoding/json` находится снаружи нашего пакета, поэтому он "не видит" эти поля и не может их сериализовать.

### Как исправить:
Назвать поля с Большой Буквы и добавить теги при необходимости:
```go
type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
```

</details>

---

## 10. Длина строки (Bytes vs Runes)

Что выведет `len` для строки с кириллицей?

```go
package main
import "fmt"

func main() {
    s := "Привет"
    fmt.Println(len(s))
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат: `12`

### Почему:
Функция `len()` для строк возвращает **количество байт**, а не количество символов.
Строки в Go — это UTF-8. Кириллические символы обычно занимают 2 байта.
6 букв * 2 байта = 12.

### Как получить количество символов:
Нужно привести строку к срезу рун:
```go
fmt.Println(len([]rune(s))) // Выведет 6
// Или использовать пакет utf8
// utf8.RuneCountInString(s)
```

</details>

---

## 11. Variable Shadowing (Затенение)

Какое число выведет `fmt.Println(x)`?

```go
package main
import "fmt"

func main() {
    x := 10
    if x > 5 {
        x := 5 // <-- Внимательно тут
        fmt.Println(x)
    }
    fmt.Println(x)
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат:
```
5
10
```

### Почему:
Внутри блока `if` мы использовали `x := 5`. Оператор `:=` создает **новую локальную переменную**, которая затеняет переменную `x` из внешней области видимости.
Внешняя `x` (которая 10) не изменилась.

### Как исправить (если хотели изменить):
Использовать `=` вместо `:=`:
```go
x = 5
```

</details>

---

## 12. Форматирование времени (Magic Date)

Что не так с форматом даты?

```go
package main
import (
    "fmt"
    "time"
)

func main() {
    t := time.Now()
    fmt.Println(t.Format("2024-01-01 15:00:00"))
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ошибка: Неверный вывод даты

### Почему:
В Go для форматирования используется конкретная "магическая дата": **Mon Jan 2 15:04:05 MST 2006**.
Если вы напишете `2024-01-01`, Go просто расставит числа в хаотичном порядке, пытаясь угадать паттерн.

### Как исправить:
Нужно использовать числа из эталонной даты:
*   Год: 2006
*   Месяц: 01
*   День: 02
*   Час: 15 (или 03)
*   Минута: 04
*   Секунда: 05

```go
t.Format("2006-01-02 15:04:05")
```

</details>

---

## 13. Goroutine Leak (Утечка горутин)

Почему этот код вызывает утечку памяти, если `process` вернет ошибку?

```go
func process() error { return nil }

func handler() {
    ch := make(chan int) // Unbuffered!
    go func() {
        val := 10
        ch <- val // Блокировка здесь
    }()

    if err := process(); err != nil {
        return // Выход без чтения из канала
    }
    <-ch
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Проблема: Горутина зависнет навсегда

### Почему:
Канал `ch` небуферизированный. Запись в него `ch <- val` блокирует горутину, пока кто-то не прочитает из канала.
Если `process()` вернет ошибку, функция `handler` завершится (`return`), и строка `<-ch` никогда не выполнится.
Горутина останется висеть в памяти, пытаясь записать в канал, который никто не читает.

### Как исправить:
Сделать канал буферизированным `make(chan int, 1)`, чтобы горутина могла записать значение и завершиться, даже если никто не читает.

</details>

---

## 14. Сравнение структур

Почему этот код не скомпилируется?

```go
package main
import "fmt"

type Data struct {
    Nums []int
}

func main() {
    d1 := Data{Nums: []int{1, 2}}
    d2 := Data{Nums: []int{1, 2}}
    
    if d1 == d2 {
        fmt.Println("Equal")
    }
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ошибка: struct containing []int cannot be compared

### Почему:
Структуры в Go можно сравнивать через `==`, только если **все их поля сравнимы**.
Слайсы (`[]int`) **несравнимы** (их можно сравнивать только с `nil`). Поскольку структура содержит слайс, сама структура тоже теряет возможность сравнения.

### Как исправить:
Использовать `reflect.DeepEqual` (медленно) или написать свою функцию сравнения, или использовать `slices.Equal` (Go 1.21+).

</details>

---

## 15. Recover вне Defer

Почему паника не будет перехвачена?

```go
package main
import "fmt"

func main() {
    if r := recover(); r != nil {
        fmt.Println("Recovered:", r)
    }
    panic("BOOM")
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат: Crash (Panic)

### Почему:
`recover()` работает **только** если он вызывается внутри отложенной функции (`defer`).
Вызов `recover()` напрямую в теле функции ничего не делает (возвращает `nil`).
Когда случается `panic`, программа прекращает нормальное выполнение и начинает выполнять только `defer`-функции.

### Как исправить:
```go
defer func() {
    if r := recover(); r != nil {
        fmt.Println("Recovered:", r)
    }
}()
panic("BOOM")
```

</details>

## 16. Defer и аргументы функции

Что выведет код и почему время одинаковое (или разное)?

```go
package main
import (
    "fmt"
    "time"
)

func main() {
    fmt.Println("Start")
    defer fmt.Println("End:", time.Now())
    
    time.Sleep(2 * time.Second)
    fmt.Println("Main finished")
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат:
Время в "End" будет таким же (почти), как и при "Start".

### Почему:
Аргументы отложенной функции вычисляются **в момент вызова defer**, а не в момент выхода из функции.
`time.Now()` выполнился сразу, когда программа дошла до строки с `defer`, результат сохранился, и именно он будет выведен в конце.

### Как исправить (чтобы вывести время окончания):
Обернуть в анонимную функцию (замыкание):
```go
defer func() {
    fmt.Println("End:", time.Now())
}()
```
Здесь `time.Now()` выполнится только когда вызовется анонимная функция.

</details>

---

## 17. Pointer Receiver и Interface

Скомпилируется ли этот код?

```go
package main

type Animal interface {
    Speak() string
}

type Cat struct{}

func (c *Cat) Speak() string { // Обрати внимание на указатель
    return "Meow"
}

func main() {
    var a Animal
    a = Cat{} // Присваиваем значение (не указатель)
    _ = a.Speak()
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ошибка: Compilation Error

### Почему:
Метод `Speak` определен для типа `*Cat` (указатель), а не для `Cat` (значение).
Тип `Cat` не реализует интерфейс `Animal`, его реализует только `*Cat`.
Значение `Cat{}` неадресуемо при присваивании в интерфейс напрямую (копия значения не имеет метода Speak).

### Как исправить:
Присваивать указатель:
```go
a = &Cat{}
```

</details>

---

## 18. Утечка памяти в слайсах (Slice Memory Leak)

В чем проблема этой функции, если файл очень большой?

```go
var savedData []byte

func saveChunk(data []byte) {
    // Допустим, data весит 100 MB
    // Нам нужны только первые 10 байт
    savedData = data[:10]
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Проблема: Утечка памяти

### Почему:
Слайс `savedData` указывает на тот же **базовый массив**, что и `data`.
Даже если мы видим только 10 байт, в памяти GC не может удалить весь массив на 100 MB, так как на него всё ещё есть ссылка через `savedData`.

### Как исправить:
Сделать копию данных в новый независимый слайс:
```go
func saveChunk(data []byte) {
    chunk := make([]byte, 10)
    copy(chunk, data[:10])
    savedData = chunk
}
```

</details>

---

## 19. Порядок итерации Map

Что гарантирует этот код по порядку вывода?

```go
package main
import "fmt"

func main() {
    m := map[string]int{"a": 1, "b": 2, "c": 3, "d": 4}
    for k, v := range m {
        fmt.Printf("%s:%d ", k, v)
    }
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ответ: Ничего. Порядок случайный.

### Почему:
В Go итерация по `map` специально сделана **рандомизированной**. При каждом запуске программы (или даже цикла) порядок ключей может отличаться. Это сделано, чтобы программисты не полагались на порядок ключей, так как хеш-таблицы его не гарантируют.

### Как исправить:
Если нужен порядок, соберите ключи в слайс, отсортируйте их и итерируйтесь по слайсу:
```go
keys := make([]string, 0, len(m))
for k := range m { keys = append(keys, k) }
sort.Strings(keys)
for _, k := range keys { fmt.Println(k, m[k]) }
```

</details>

---

## 20. Type Assertion Panic

Что произойдет, если `i` не является строкой?

```go
package main
import "fmt"

func main() {
    var i interface{} = 10
    s := i.(string) // <-- Опасно
    fmt.Println(s)
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат: Panic

### Почему:
Прямое утверждение типа `x.(T)` вызывает панику, если интерфейс хранит не `T`.

### Как исправить:
Использовать форму с двумя возвращаемыми значениями ("comma ok idiom"):
```go
s, ok := i.(string)
if !ok {
    fmt.Println("Not a string")
} else {
    fmt.Println(s)
}
```

</details>

---

## 21. JSON: Null vs Empty Slice

В чем разница в JSON между этими двумя структурами?

```go
type Response struct {
    Users []string `json:"users"`
}

func main() {
    var r1 Response         // Случай 1
    r2 := Response{Users: []string{}} // Случай 2
    // ... marshal ...
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ответ: `null` vs `[]`

### Почему:
1. `r1.Users` — это `nil` слайс (дефолтное значение). В JSON он превратится в **`null`**.
2. `r2.Users` — это инициализированный, но пустой слайс. В JSON он превратится в **`[]`**.

Для фронтенда это может быть критично (ошибка `Cannot map null` vs пустой список).

</details>

---

## 22. Switch Fallthrough

Что выведет этот код?

```go
package main
import "fmt"

func main() {
    val := 10
    switch val {
    case 10:
        fmt.Println("Ten")
        fallthrough
    case 20:
        fmt.Println("Twenty")
    default:
        fmt.Println("Default")
    }
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Результат:
```
Ten
Twenty
```

### Почему:
В Go `switch` не требует `break` (в отличие от C/C++), он выходит автоматически после выполнения кейса.
Ключевое слово `fallthrough` принудительно заставляет выполнить код **следующего** кейса (даже без проверки условия).

</details>

---

## 23. Сравнение времени (Monotonic Clock)

Почему `t1` может быть не равно `t2`, даже если время одинаковое?

```go
t1 := time.Now()
t2 := t1.Round(0) // Или получено из JSON/БД
// ...
if t1 == t2 { ... }
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Проблема: Монотонные часы

### Почему:
`time.Now()` в Go содержит два значения:
1. Wall clock (календарное время).
2. Monotonic clock (время работы процессора, для измерения длительности).

Когда мы сериализуем время (в БД или JSON) или округляем его, Monotonic-часть отбрасывается.
Оператор `==` сравнивает обе части.
`t1` (с monotonic) != `t2` (без monotonic), даже если на часах одно и то же время.

### Как исправить:
Использовать метод `.Equal()` или `.Truncate()`:
```go
if t1.Equal(t2) { ... }
```

</details>

---

## 24. Эффективная конкатенация строк

Почему этот код плох для больших циклов?

```go
var s string
for i := 0; i < 10000; i++ {
    s += "hello"
}
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Проблема: Низкая производительность (O(n^2))

### Почему:
Строки в Go неизменяемы (immutable).
Каждая операция `+=` создает **новую** строку, копируя старое содержимое + новое. Это создает огромную нагрузку на Garbage Collector и память.

### Как исправить:
Использовать `strings.Builder` (аналог StringBuilder):
```go
var sb strings.Builder
for i := 0; i < 10000; i++ {
    sb.WriteString("hello")
}
s := sb.String()
```

</details>

---

## 25. Init Function Order

В каком порядке выполняются функции `init`?

```go
// main.go imports "pkgA"
// "pkgA" imports "pkgB"
```
Кто инициализируется первым?

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ответ: Снизу вверх (зависимости первыми)

### Порядок:
1. `pkgB` init()
2. `pkgA` init()
3. `main` init()
4. `main` main()

Go сначала инициализирует все импортируемые пакеты (рекурсивно), и только потом текущий.

</details>

---

## 26. Break из вложенного цикла

Как выйти из внешнего цикла `ForLoop`?

```go
func main() {
    ForLoop: // Метка
    for i := 0; i < 5; i++ {
        for j := 0; j < 5; j++ {
            if i*j > 10 {
                break // <-- Выходит только из внутреннего
            }
        }
    }
}
```
*Что нужно изменить?*

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ответ: Использовать Labels (метки)

Обычный `break` прерывает только самый внутренний цикл `for`, `switch` или `select`.
Чтобы выйти из внешнего, нужно указать метку:
```go
break ForLoop
```

</details>

---

## 27. String Immutability

Что будет, если попытаться изменить символ строки?

```go
s := "hello"
s[0] = 'H'
```

<details>
<summary><b>🔍 Показать ответ</b></summary>

### Ошибка: Compilation Error

### Почему:
Строки в Go неизменяемы. Нельзя присвоить значение по индексу.

### Как исправить:
Преобразовать в `[]byte` или `[]rune`, изменить, и преобразовать обратно:
```go
bytes := []byte(s)
bytes[0] = 'H'
s = string(bytes)
```

</details>
