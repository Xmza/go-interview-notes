# 🟢 Testing

* [Как устроен пакет testing](#как-устроен-пакет-testing)
* [Что такое tHelper](#что-такое-thelper)
* [Как писать table-driven-tests](#как-писать-table-driven-tests)
* [Как замерять производительность go-test--bench](#как-замерять-производительность-go-test--bench)
* [Что такое race-detector go-test--race](#что-такое-race-detector-go-test--race)

---

## Как устроен пакет testing

`testing` — это встроенный пакет для написания unit-тестов, бенчмарков и примеров.

* Файлы тестов: `*_test.go`
* Функции тестов: `func TestXxx(t *testing.T)`
* Бенчмарки: `func BenchmarkXxx(b *testing.B)`
* Примеры: `func ExampleXxx()`

При запуске `go test`:

1. Компилятор собирает бинарь с тестами.
2. Запускается тестовый раннер, который ищет все функции `Test*`, `Benchmark*`, `Example*`.
3. Каждый тест выполняется изолированно, с отчётами об ошибках и временем выполнения.

---

## Что такое t.Helper

`t.Helper()` помечает функцию как «вспомогательную» для тестов.

* Без `t.Helper()` ошибки будут указывать на саму вспомогательную функцию.
* С `t.Helper()` ошибки будут указывать на строку, где вызван helper.

Пример:

```go
func checkErr(t *testing.T, err error) {
    t.Helper() // теперь ошибка укажет на место вызова checkErr, а не сюда
    if err != nil {
        t.Fatal(err)
    }
}
```

---

## Как писать table-driven-tests

Table-driven tests — популярный паттерн в Go.

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"1+1", 1, 1, 2},
        {"2+3", 2, 3, 5},
        {"-1+1", -1, 1, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Errorf("got %d, want %d", got, tt.want)
            }
        })
    }
}
```

Плюсы: читаемость, расширяемость, возможность параллелизации (`t.Parallel()`).

---

## Как замерять производительность go-test--bench

Для бенчмарков используется:

```bash
go test -bench .
```

Формат функции:

```go
func BenchmarkXxx(b *testing.B) {
    for i := 0; i < b.N; i++ {
        myFunc()
    }
}
```

* `b.N` автоматически подбирается раннером.
* Используются метрики: ns/op, B/op, allocs/op.
* Для замеров памяти: `b.ReportAllocs()`.

---

## Что такое race-detector go-test--race

Race detector обнаруживает **data race** — одновременный доступ к памяти из нескольких горутин без синхронизации.

Запуск:

```bash
go test -race ./...
```

Пример проблемы:

```go
var counter int

func main() {
    go func() { counter++ }()
    go func() { counter++ }()
}
```

Race detector укажет строки с конфликтами.

<pre>
В продакшене флаг `-race` замедляет выполнение, но в тестах он must-have.
</pre>
