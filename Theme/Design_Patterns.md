# 📕 Design Patterns (Go Idioms)

- [Паттерн Functional Options (Конфигурация объектов)](#паттерн-functional-options-конфигурация-объектов)
- [Паттерн Singleton (через sync.Once)](#паттерн-singleton-через-synconce)
- [Паттерн Builder и Factory Method в Go](#паттерн-builder-и-factory-method-в-go)
- [Паттерн Adapter (Использование интерфейсов для подмены реализаций)](#паттерн-adapter-использование-интерфейсов-для-подмены-реализаций)
- [Паттерн Strategy (Смена поведения в рантайме)](#паттерн-strategy-смена-поведения-в-рантайме)
- [Паттерн Worker Pool (Реализация на каналах)](#паттерн-worker-pool-реализация-на-каналах)
- [Паттерн Pipeline (Конвейер обработки данных)](#паттерн-pipeline-конвейер-обработки-данных)
- [Паттерн Circuit Breaker (для микросервисов)](#паттерн-circuit-breaker-для-микросервисов)

---

## Паттерн Functional Options (Конфигурация объектов)

Идиоматичный способ создания объектов с опциональными параметрами (вместо кучи конструкторов или config-структур).

```go
type Server struct {
    Port int
    Timeout time.Duration
}

type Option func(*Server)

func WithPort(port int) Option {
    return func(s *Server) {
        s.Port = port
    }
}

func NewServer(opts ...Option) *Server {
    srv := &Server{Port: 8080} // Дефолт
    for _, opt := range opts {
        opt(srv)
    }
    return srv
}

// Использование:
s := NewServer(WithPort(9000))
```

<pre>
Functional Options позволяет иметь красивые и расширяемые API инициализации.
</pre>

---

## Паттерн Singleton (через sync.Once)

Гарантирует, что объект создан ровно один раз. В Go реализуется через `sync.Once`. Это потокобезопасно.

```go
var instance *Database
var once sync.Once

func GetDatabase() *Database {
    once.Do(func() {
        instance = &Database{} // Выполнится только 1 раз, даже если вызовут 100 горутин
    })
    return instance
}
```

---

## Паттерн Builder и Factory Method в Go

*   **Factory:** Обычная функция `New...`, которая возвращает интерфейс.
    ```go
    func NewStorage(type string) (Storage, error) {
        if type == "disk" { return &DiskStorage{}, nil }
        return &MemStorage{}, nil
    }
    ```
*   **Builder:** Используется редко, так как Functional Options покрывает 90% кейсов. Нужен, когда процесс создания объекта очень сложный и многоступенчатый.

---

## Паттерн Adapter (Использование интерфейсов для подмены реализаций)

Позволяет использовать структуры с несовместимыми интерфейсами вместе.
Пример: У нас есть метод `Process(r Reader)`, а у нас есть старая структура `LegacyReader`, у которой метод называется `ReadData`.

Мы создаем обертку (Adapter), которая реализует `Reader`, а внутри вызывает `legacy.ReadData`.

<pre>
В Go адаптеры встречаются повсеместно при работе со стандартными интерфейсами io.Reader, io.Writer.
</pre>

---

## Паттерн Strategy (Смена поведения в рантайме)

Позволяет выбирать алгоритм действий во время выполнения.
В Go это просто **поле типа интерфейс** внутри структуры.

```go
type Logger struct {
    Writer io.Writer // Стратегия: куда писать (файл, консоль, сеть)
}

func (l *Logger) Log(msg string) {
    l.Writer.Write([]byte(msg)) // Логгер не знает, куда пишет, он делегирует это Стратегии
}
```

---

## Паттерн Worker Pool (Реализация на каналах)

Паттерн для ограничения конкурентности. Вместо создания 1000 горутин на 1000 задач, мы создаем 5 воркеров, которые разгребают очередь.

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Println("worker", id, "processing", j)
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // Запускаем 3 воркера
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    // Шлем задачи
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)
    // ... чтение результатов
}
```

---

## Паттерн Pipeline (Конвейер обработки данных)

Цепочка этапов обработки, где выход одного этапа является входом для другого (через каналы).

`Generator -> Channel -> Square -> Channel -> Print`

Это позволяет обрабатывать потоковые данные (стримы), не загружая всё в память.

---

## Паттерн Circuit Breaker (для микросервисов)

"Автомат-предохранитель". Если внешний сервис начал отдавать ошибки (например, БД упала), Circuit Breaker "размыкается" и сразу отдает ошибку, не пытаясь делать реальный запрос.
Это спасает систему от каскадного отказа (когда все потоки висят в ожидании ответа от мертвого сервиса).
Через некоторое время он пропускает 1 пробный запрос ("Half-Open"), и если ок — замыкается обратно.
*Библиотека:* `gobreaker`.
