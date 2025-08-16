Context

Time

Patterns

Concurrency паттерны в Go

Подробнее про fan-in fan-out

cache contention

Локальная и глобальная очередь горутин или тредов 

 I/O-Bound

🟢 Memory management & runtime

Как устроен GC в Go (алгоритм, поколения, stop-the-world и т.д.).

Что такое escape analysis (как работает go build -gcflags="-m", почему переменная уходит в heap).

Как устроен heap vs stack в Go.

Что такое zero value и зачем Go так сделан.

Что такое arena allocator (новое в Go 1.22+).

🟢 Concurrency & parallelism

Чем отличается параллелизм от конкурентности.

Сколько по умолчанию горутин может выполняться параллельно (GOMAXPROCS).

Как работает work stealing в планировщике.

Что будет если main завершится, а горутины продолжают работать.

Что такое deadlock и как его поймать.

Что такое livelock, чем отличается от deadlock.

Чем отличается blocking vs non-blocking канал.

🟢 Language details

Что такое defer, порядок выполнения, оптимизации компилятора.

Как работает init(), порядок вызова в пакете и между пакетами.

Чем отличается new и make.

Что произойдет, если сравнивать слайсы (==).

Что такое alias типов (type A = B) vs новый тип (type A B).

Как работает copy для слайсов.

Что такое rune и byte в Go.

Что будет, если вернуть из функции ссылку на локальную переменную.

🟢 Testing

Как устроен пакет testing.

Что такое t.Helper().

Как писать table-driven tests.

Как замерять производительность (go test -bench).

Что такое race detector (go test -race).

🟢 Tools & ecosystem

Что делает go mod tidy, go mod vendor, go mod graph.

Как устроен go build, какие этапы компиляции.

Что делает go generate.

Как работает go fmt и go vet.

Что такое pprof и как с ним работать.

🟢 Network & system

Как работает context.Context, зачем нужен.

Чем отличается context.WithCancel vs WithDeadline vs WithTimeout.

Что будет, если не вызывать cancel().

Что будет, если закрыть http.Response.Body не вызвать.

Как устроен net/http сервер под капотом (goroutines-per-connection).

🟢 Advanced (любят на мидл+)

Как устроен reflect и зачем он нужен.

Что такое generics в Go, какие ограничения (type constraints).

Чем отличаются interface{} vs any (Go 1.18+).

Что будет, если в Go сделать циклическую зависимость пакетов.

Как работает unsafe.Pointer, когда его использовать.

Что такое atomic.Value и когда использовать вместо мьютекса.

Как устроен sync.Once.

Зачем нужен build tags (//go:build).
