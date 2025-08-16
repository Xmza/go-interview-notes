# 🟢 Microservices & Integration

* [Как Go используют в микросервисной архитектуре](#как-go-используют-в-микросервисной-архитектуре)
* [Как общаются микросервисы gRPC vs REST](#как-общаются-микросервисы-grpc-vs-rest)
* [Как работать с Kafka из Go](#как-работать-с-kafka-из-go)
* [Как работать с Redis в Go](#как-работать-с-redis-в-go)
* [Как правильно организовать graceful shutdown микросервиса](#как-правильно-организовать-graceful-shutdown-микросервиса)

---

## Как Go используют в микросервисной архитектуре

Go популярен для микросервисов, потому что:

* **Быстрая компиляция в статический бинарь** → легко деплоить в Docker/K8s;
* **Малое потребление памяти и быстрый runtime** → сервисы держат тысячи коннекшенов;
* **Отличная поддержка concurrency** → удобно писать сетевые сервисы;
* **Стандартная библиотека** уже покрывает HTTP, JSON, gRPC, pprof и многое другое.

Go часто применяют для:

* API-шлюзов;
* сервисов реального времени (WebSockets, SSE);
* воркеров для обработки событий.

---

## Как общаются микросервисы gRPC vs REST

* **REST** — проще для интеграции, основан на HTTP/JSON, легко дебажить, но медленнее и многословнее;
* **gRPC** — бинарный протокол (HTTP/2 + Protobuf), быстрый, поддерживает стримы и bidirectional RPC, но требует генерации кода.

Go имеет первую классную поддержку gRPC через `google.golang.org/grpc`.

---

## Как работать с Kafka из Go

Основные клиенты:

* [`confluent-kafka-go`](https://github.com/confluentinc/confluent-kafka-go) — быстрый, обертка над C-библиотекой `librdkafka`;
* [`segmentio/kafka-go`](https://github.com/segmentio/kafka-go) — написан чисто на Go, проще, но чуть медленнее.

Пример потребителя:

```go
r := kafka.NewReader(kafka.ReaderConfig{
    Brokers: []string{"localhost:9092"},
    Topic:   "events",
    GroupID: "service-1",
})
for {
    m, err := r.ReadMessage(context.Background())
    if err != nil { break }
    fmt.Printf("message at offset %d: %s = %s\n", m.Offset, string(m.Key), string(m.Value))
}
```

---

## Как работать с Redis в Go

Библиотека по умолчанию — [`go-redis`](https://github.com/redis/go-redis).

Пример:

```go
rdb := redis.NewClient(&redis.Options{Addr: "localhost:6379"})
ctx := context.Background()

err := rdb.Set(ctx, "key", "value", time.Minute).Err()
val, err := rdb.Get(ctx, "key").Result()
```

Redis часто используют для:

* кэша;
* pub/sub;
* хранения сессий;
* rate limiting.

---

## Как правильно организовать graceful shutdown микросервиса

1. Ловим сигналы `SIGTERM`, `SIGINT` через `os/signal`;
2. Используем `context.WithTimeout` для ограничения времени завершения;
3. Закрываем все подключения (DB, Kafka, Redis);
4. Ждём завершения горутин.

Пример:

```go
ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt, syscall.SIGTERM)
defer stop()

srv := &http.Server{Addr: ":8080", Handler: mux}

// Запускаем сервер
go func() { srv.ListenAndServe() }()

<-ctx.Done() // ждём сигнала
shutdownCtx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

srv.Shutdown(shutdownCtx)
```

<pre>
Таким образом микросервис корректно освобождает ресурсы и не роняет запросы.
</pre>
