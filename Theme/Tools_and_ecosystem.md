#  Tools & ecosystem

* [Что делает go-mod-tidy go-mod-vendor go-mod-graph](#что-делает-go-mod-tidy-go-mod-vendor-go-mod-graph)
* [Как устроен go-build какие этапы компиляции](#как-устроен-go-build-какие-этапы-компиляции)
* [Что делает go-generate](#что-делает-go-generate)
* [Как работает go-fmt-и-go-vet](#как-работает-go-fmt-и-go-vet)
* [Что такое pprof и как с ним работать](#что-такое-pprof-и-как-с-ним-работать)

---

## Что делает go mod tidy go mod vendor go mod graph

* `go mod tidy` — убирает неиспользуемые зависимости и добавляет недостающие.
* `go mod vendor` — копирует зависимости в папку `vendor/`.
* `go mod graph` — показывает граф зависимостей.

---

## Как устроен go build какие этапы компиляции

Этапы компиляции:

1. Парсинг → AST.
2. Escape analysis (stack vs heap).
3. SSA оптимизации.
4. Генерация машинного кода.
5. Линковка.

---

## Что делает go generate

`go generate` запускает генерацию кода.

В коде можно писать:

```go
//go:generate stringer -type=MyEnum
```

И потом вызвать:

```bash
go generate ./...
```

---

## Как работает go fmt и go vet

* `go fmt` — автоформатирование кода (отступы, скобки, пробелы).
* `go vet` — статический анализатор, ищет потенциальные баги (неиспользованные переменные, printf с неправильными аргументами и т.д.).

---

## Что такое pprof и как с ним работать

`pprof` — инструмент профилирования.

* CPU profiling, Memory profiling, Block, Mutex.
* Запуск:

```go
import _ "net/http/pprof"
http.ListenAndServe(":6060", nil)
```

Дальше:

```bash
go tool pprof http://localhost:6060/debug/pprof/profile
```

Профиль можно визуализировать (`web`, `svg`).
