📗 / 📕

# Шпаргалка по Golang

<details>
<summary><strong>Arrays & Slices 📗</strong></summary>

1. [Что такое слайс?](Theme/Arrays_&_Slices.md#что-такое-слайс) 
2. [Чем массив отличается от слайса? Чем хорош массив по сравнению со слайсом?](Theme/Arrays_&_Slices.md#чем-массив-отличается-от-слайса-чем-хорош-массив-по-сравнению-со-слайсом)
3. [Как работает append?](Theme/Arrays_&_Slices.md#как-работает-append)
4. [Какие методы оптимизации работы со слайсами ты бы применил в работе?](Theme/Arrays_&_Slices.md#какие-методы-оптимизации-работы-со-слайсами-ты-бы-применил-в-работе)
5. [Какая есть функции для создания слайса с длиной отличной от нуля?](Theme/Arrays_&_Slices.md#какая-есть-функции-для-создания-слайса-с-длиной-отличной-от-нуля)
6. [С какой скоростью идет поиск в массиве и почему?](Theme/Arrays_&_Slices.md#с-какой-скоростью-идет-поиск-в-массиве-и-почему)
</details>

<details>
<summary><strong>Advanced 📕</strong></summary>

1. [Как устроен reflect и зачем нужен](Theme/Advanced.md#как-устроен-reflect-и-зачем-нужен)
2. [Что такое generics в Go какие ограничения](Theme/Advanced.md#что-такое-generics-в-go-какие-ограничения)
3. [Чем отличаются interface{} vs any](Theme/Advanced.md#чем-отличаются-interface-vs-any)
4. [Что будет если сделать циклическую зависимость пакетов](Theme/Advanced.md#что-будет-если-сделать-циклическую-зависимость-пакетов)
5. [Как работает unsafePointer и когда его использовать](Theme/Advanced.md#как-работает-unsafepointer-и-когда-его-использовать)
6. [Что такое atomicValue и когда использовать вместо мьютекса](Theme/Advanced.md#что-такое-atomicvalue-и-когда-использовать-вместо-мьютекса)
7. [Как устроен syncOnce](Theme/Advanced.md#как-устроен-synconce)
8. [Зачем нужен build-tags](Theme/Advanced.md#зачем-нужен-build-tags)
</details>

<details>
<summary><strong>Channels 📗</strong></summary>

1. [Что такое каналы?](Theme/Channels.md#что-такое-каналы)
2. [Как устроен канал и как он работает под капотом?](Theme/Channels.md#как-устроен-канал-и-как-он-работает-под-капотом)
3. [Какие есть типы каналов в Golang?](Theme/Channels.md#какие-есть-типы-каналов-в-golang)
4. [Что если писать/читать в закрытый канал?](Theme/Channels.md#что-если-писатьчитать-в-закрытый-канал)
5. [Какие операции есть с каналами?](Theme/Channels.md#какие-операции-есть-с-каналами)
6. [Как сделать канал буферизованным?](Theme/Channels.md#как-сделать-канал-буферизованным)
7. [Какие параметры могут иметь каналы?](Theme/Channels.md#какие-параметры-могут-иметь-каналы)
8. [Для чего используется select при работе с каналами?](Theme/Channels.md#для-чего-используется-select-при-работе-с-каналами)
9. [Что если закрыть закрытый канал?](Theme/Channels.md#что-если-закрыть-закрытый-канал)
10. [Что произойдет с читателями/писателями если закрыть канал?](Theme/Channels.md#что-произойдет-с-читателямиписателями-если-закрыть-канал)
</details>

<details>
<summary><strong>Concurrency_&_parallelism 📕</strong></summary>

1. [Чем отличается параллелизм от конкурентности](Theme/Concurrency_&_parallelism.md#чем-отличается-параллелизм-от-конкурентности)
2. [Сколько по умолчанию горутин может выполняться параллельно (GOMAXPROCS)](Theme/Concurrency_&_parallelism.md#сколько-по-умолчанию-горутин-может-выполняться-параллельно-gomaxprocs)
3. [Как работает work stealing в планировщике](Theme/Concurrency_&_parallelism.md#как-работает-work-stealing-в-планировщике)
4. [Что будет если main завершится а горутины продолжают работать](Theme/Concurrency_&_parallelism.md#что-будет-если-main-завершится-а-горутины-продолжают-работать)
5. [Что такое deadlock и как его поймать](Theme/Concurrency_&_parallelism.md#что-такое-deadlock-и-как-его-поймать)
6. [Что такое livelock чем отличается от-deadlock](Theme/Concurrency_&_parallelism.md#что-такое-livelock-чем-отличается-от-deadlock)
7. [Чем отличается blocking-vs-non-blocking-канал](Theme/Concurrency_&_parallelism.md#чем-отличается-blocking-vs-non-blocking-канал)
</details>

<details>
<summary><strong>Exception and panic 📗</strong></summary>

1. [recover происходит только в той горутине где произошла паника](Theme/Exception_and_panic.md#recover-происходит-только-в-той-горутине-где-произошла-паника)
2. [Чем отличается работа с ошибками в Golang от других языков?](Theme/Exception_and_panic.md#чем-отличается-работа-с-ошибками-в-golang-от-других-языков)
3. [Какая парадигма в Golang с точки зрения обработки исключений и ошибок?](Theme/Exception_and_panic.md#какая-парадигма-в-golang-с-точки-зрения-обработки-исключений-и-ошибок)
4. [Какие есть функции для оборачивания и сравнения ошибок?](Theme/Exception_and_panic.md#какие-есть-функции-для-оборачивания-и-сравнения-ошибок)
5. [Для чего используется паника?](Theme/Exception_and_panic.md#для-чего-используется-паника)
</details>

<details>
<summary><strong>Goroutines 📗</strong></summary>

1. [Что такое горутина?](Theme/Goroutines.md#что-такое-горутина)
2. [Чем горутина отличается от треда?](Theme/Goroutines.md#чем-горутина-отличается-от-треда)
3. [В чем преимущества горутин над тредами?](Theme/Goroutines.md#в-чем-преимущества-горутин-над-тредами)
4. [Что есть в Golang для многопоточности?](Theme/Goroutines.md#что-есть-в-golang-для-многопоточности)
5. [Какие есть способы связи между горутинами, какие плюсы и минусы?](Theme/Goroutines.md#какие-есть-способы-связи-между-горутинами-какие-плюсы-и-минусы)
</details>

<details>
<summary><strong>Interface 📗</strong></summary>

1. [Что такое интерфейс?](Theme/Interface.md#что-такое-интерфейс)
2. [Для чего используются интерфейсы и как они устроены?](Theme/Interface.md#для-чего-используются-интерфейсы-и-как-они-устроены)
3. [Зачем нужен пустой интерфейс?](Theme/Interface.md#зачем-нужен-пустой-интерфейс)
4. [Чем пустой интерфейс отличается от пустой структуры?](Theme/Interface.md#чем-пустой-интерфейс-отличается-от-пустой-структуры)
5. [Есть интерфейс, а есть указатель на структуру, который nil. Кладем указатель в интерфейс. Что если сравнить интерфейс с nil?](Theme/Interface.md#есть-интерфейс-а-есть-указатель-на-структуру-который-nil-кладем-указатель-в-интерфейс-что-если-сравнить-интерфейс-с-nil)
</details>

<details>
<summary><strong>Language details 📕</strong></summary>

1. [Что такое defer, порядок выполнения, оптимизации компилятора](Theme/Language_details.md#что-такое-defer-порядок-выполнения-оптимизации-компилятора)
2. [Как работает init(), порядок вызова в пакете и между пакетами](Theme/Language_details.md#как-работает-init-порядок-вызова-в-пакете-и-между-пакетами)
3. [Чем отличается new и make](Theme/Language_details.md#чем-отличается-new-и-make)
4. [Что произойдет если сравнивать слайсы](Theme/Language_details.md#что-произойдет-если-сравнивать-слайсы)
5. [Что такое alias типов (type-a--b) vs новый тип (type-a-b)](Theme/Language_details.md#что-такое-alias-типов-type-a--b-vs-новый-тип-type-a-b)
6. [Как работает copy для слайсов](Theme/Language_details.md#как-работает-copy-для-слайсов)
7. [Что такое rune и byte в Go](Theme/Language_details.md#что-такое-rune-и-byte-в-go)
8. [Что будет если вернуть из функции ссылку на локальную переменную](Theme/Language_details.md#что-будет-если-вернуть-из-функции-ссылку-на-локальную-переменную)
</details>

<details>
<summary><strong>MAP 📗</strong></summary>

1. [Что такое мапа?](Theme/MAP.md#что-такое-мапа)
2. [Как устроена мапа под капотом?](Theme/MAP.md#как-устроена-мапа-под-капотом)
3. [Какие ключи могут быть у мапы?](Theme/MAP.md#какие-ключи-могут-быть-у-мапы)
4. [Что произойдет при конкуррентной записи в мапу?](Theme/MAP.md#что-произойдет-при-конкуррентной-записи-в-мапу)
5. [Как работает эвакуация данных?](Theme/MAP.md#как-работает-эвакуация-данных)
6. [Потокобезопасная ли мапа?](Theme/MAP.md#потокобезопасная-ли-мапа)
7. [Map vs Sync.Map](Theme/MAP.md#map-vs-syncmap)
8. [Какая сложность работы с мапой?](Theme/MAP.md#какая-сложность-работы-с-мапой)
9. [Можно ли взять адрес элемента мапы и почему?](Theme/MAP.md#можно-ли-взять-адрес-элемента-мапы-и-почему)
</details>

<details>
<summary><strong>Memory_managment 📕</strong></summary>
  
1. [Как устроен GC в Go (алгоритм, поколения, stop-the-world и т.д.)](Theme/Memory_managment.md#как-устроен-GC-в-Go)
2. [Что такое escape analysis (как работает go-build--gcflags-m-почему-переменная-уходит-в-heap)](Theme/Memory_managment.md#что-такое-escape-analysis)
3. [Как устроен heap vs stack в Go](Theme/Memory_managment.md#как-устроен-heap-vs-stack-в-Go)
4. [Что такое zero value и зачем Go так сделан](Theme/Memory_managment.md#что-такое-zero-value-и-зачем-Go-так-сделан)
5. [Что такое arena allocator (новое в Go 122)](Theme/Memory_managment.md#что-такое-arena-allocator)
</details>

<details>
<summary><strong>Network_and_system 📕</strong></summary>
  
1. [Как работает context.Context и зачем нужен](Theme/Network_and_system.md#как-работает-contextcontext-и-зачем-нужен)
2. [Чем отличается contextWithCancel vs WithDeadline vs WithTimeout](Theme/Network_and_system.md#чем-отличается-contextwithcancel-vs-withdeadline-vs-withtimeout)
3. [Что будет если не вызывать cancel](Theme/Network_and_system.md#что-будет-если-не-вызывать-cancel)
4. [Что будет если закрыть httpResponseBody не вызвать](Theme/Network_and_system.md#что-будет-если-закрыть-httpresponsebody-не-вызвать)
5. [Как устроен nethttp-сервер под капотом](Theme/Network_and_system.md#как-устроен-nethttp-сервер-под-капотом)
</details>

<details>
<summary><strong>OOP 📗</strong></summary>

1. [Как устроено ООП в Golang?](Theme/OOP.md#как-устроено-ооп-в-golang)
2. [Как реализуются принципы (наследование, абстракция, инкапсуляция, полиморфизм)?](Theme/OOP.md#как-устроено-ооп-в-golang)
3. [Как реализуется наследование в Golang?](Theme/OOP.md#как-реализуется-наследование-в-golang)

</details>

<details>
<summary><strong>Planner 📗</strong></summary>

1. [Как работает планировщик в Golang?](Theme/Planner.md#как-работает-планировщик-в-golang)
2. [Как работает вытесняющая многозадачность?](Theme/Planner.md#как-работает-вытесняющая-многозадачность)
3. [За счет чего достигается параллельное выполнение в Golang?](Theme/Planner.md#за-счет-чего-достигается-параллельное-выполнение-в-golang)
4. [В чем разница между вытесняющим и кооперативным планировщиком?](Theme/Planner.md#в-чем-разница-между-вытесняющим-и-кооперативным-планировщиком)
5. [Сколько потоков операционной системы мы можем создать?](Theme/Planner.md#сколько-потоков-операционной-системы-мы-можем-создать)
6. [В планировщике до версии 1.15 какие операции приводят к переключению контекста горутин?](Theme/Planner.md#в-планировщике-до-версии-115-какие-операции-приводят-к-переключению-контекста-горутин)
</details>

<details>
<summary><strong>Primitives of synchronization 📗</strong></summary>

1. [Какие примитивы синхронизации есть в Golang?](Theme/Primitives_of_synchronization.md#какие-примитивы-синхронизации-есть-в-golang)
2. [Чем мьютекс отличается от семафора?](Theme/Primitives_of_synchronization.md#чем-мьютекс-отличается-от-семафора)
3. [Чем мьютексы отличаются от атомиков?](Theme/Primitives_of_synchronization.md#чем-мьютексы-отличаются-от-атомиков)
4. [Какие примитивы синхронизации использовал в работе и для чего?](Theme/Primitives_of_synchronization.md#какие-примитивы-синхронизации-использовал-в-работе-и-для-чего)
5. [Как устроена WaitGroup под капотом и как ее можно реализовать самому?](Theme/Primitives_of_synchronization.md#как-устроена-waitgroup-под-капотом-и-как-ее-можно-реализовать-самому)
6. [Когда нужно использовать Mutex, а когда RWMutex?](Theme/Primitives_of_synchronization.md#когда-нужно-использовать-mutex-а-когда-rwmutex)
7. [Расскажи про sync.Map](Theme/Primitives_of_synchronization.md#расскажи-про-syncmap)
8. [Расскажи про пакет sync](Theme/Primitives_of_synchronization.md#расскажи-про-пакет-sync)
9. [Есть общий ресурс. Хотим, чтобы к нему одновременно обращались только N горутин. Как это сделать?](Theme/Primitives_of_synchronization.md#есть-общий-ресурс-хотим-чтобы-к-нему-одновременно-обращались-только-n-горутин-как-это-сделать)
</details>

<details>
<summary><strong>Race condition 📗</strong></summary>

1. [Что такое race condition?](Theme/Race_condition.md#что-такое-race-condition)
2. [Как обнаружить race condition?](Theme/Race_condition.md#как-обнаружить-race-condition)
3. [Какие есть способы устранения race condition?](Theme/Race_condition.md#какие-есть-способы-устранения-race-condition)
</details>

<details>
<summary><strong>Testing 📕</strong></summary>

1. [Как устроен пакет testing](Theme/Testing.md#как-устроен-пакет-testing)
2. [Что такое tHelper](Theme/Testing.md#что-такое-thelper)
3. [Как писать table-driven-tests](Theme/Testing.md#как-писать-table-driven-tests)
4. [Как замерять производительность go-test--bench](Theme/Testing.md#как-замерять-производительность-go-test--bench)
5. [Что такое race-detector go-test--race](Theme/Testing.md#что-такое-race-detector-go-test--race)
</details>

<details>
<summary><strong>Tools_and_ecosystem 📕</strong></summary>

1. [Что делает go-mod-tidy go-mod-vendor go-mod-graph](Theme/Tools_and_ecosystem.md#что-делает-go-mod-tidy-go-mod-vendor-go-mod-graph)
2. [Как устроен go-build какие этапы компиляции](Theme/Tools_and_ecosystem.md#как-устроен-go-build-какие-этапы-компиляции)
3. [Что делает go-generate](Theme/Tools_and_ecosystem.md#что-делает-go-generate)
4. [Как работает go-fmt-и-go-vet](Theme/Tools_and_ecosystem.md#как-работает-go-fmt-и-go-vet)
5. [Что такое pprof и как с ним работать](Theme/Tools_and_ecosystem.md#что-такое-pprof-и-как-с-ним-работать)
</details>
