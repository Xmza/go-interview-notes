📗 / 📕

# Шпаргалка по Golang

<details>
<summary><strong>Arrays & Slices</strong></summary> 📗

1. [Что такое слайс?](Theme/Arrays_&_Slices.md#что-такое-слайс) 
2. [Чем массив отличается от слайса? Чем хорош массив по сравнению со слайсом?](Theme/Arrays_&_Slices.md#чем-массив-отличается-от-слайса)
3. [Как работает append?](Theme/Arrays_&_Slices.md#как-работает-append)
4. [Какие методы оптимизации работы со слайсами ты бы применил в работе?](Theme/Arrays_&_Slices.md#методы-оптимизации)
5. [Какая есть функции для создания слайса с длиной отличной от нуля?](Theme/Arrays_&_Slices.md#функции-создания-слайса)
6. [С какой скоростью идет поиск в массиве и почему?](Theme/Arrays_&_Slices.md#скорость-поиска)
</details>

<details>
<summary><strong>Channels</strong></summary> 📗

1. [Что такое каналы?](Theme/Channels.md#что-такое-каналы)
2. [Как устроен канал и как он работает под капотом?](Theme/Channels.md#устройство-канала)
3. [Какие есть типы каналов в Golang?](Theme/Channels.md#типы-каналов)
4. [Что если писать/читать в закрытый канал?](Theme/Channels.md#закрытый-канал)
5. [Какие операции есть с каналами?](Theme/Channels.md#операции-с-каналами)
6. [Как сделать канал буферизованным?](Theme/Channels.md#буферизованный-канал)
7. [Какие параметры могут иметь каналы?](Theme/Channels.md#параметры-каналов)
8. [Для чего используется select при работе с каналами?](Theme/Channels.md#select-с-каналами)
9. [Что если закрыть закрытый канал?](Theme/Channels.md#двойное-закрытие)
10. [Что произойдет с читателями/писателями если закрыть канал?](Theme/Channels.md#читатели-писатели)
</details>

<details>
<summary><strong>Exception and panic</strong></summary> 📗

1. [recover происходит только в той горутине где произошла паника](Theme/Exception_and_panic.md#recover-горутина)
2. [Чем отличается работа с ошибками в Golang от других языков?](Theme/Exception_and_panic.md#ошибки-go)
3. [Какая парадигма в Golang с точки зрения обработки исключений и ошибок?](Theme/Exception_and_panic.md#парадигма-ошибок)
4. [Какие есть функции для оборачивания и сравнения ошибок?](Theme/Exception_and_panic.md#функции-ошибок)
5. [Для чего используется паника?](Theme/Exception_and_panic.md#использование-panic)
</details>

<details>
<summary><strong>Goroutines</strong></summary> 📗

1. [Что такое горутина?](Theme/Goroutines.md#горутина)
2. [Чем горутина отличается от треда?](Theme/Goroutines.md#горутина-тред)
3. [В чем преимущества горутин над тредами?](Theme/Goroutines.md#преимущества-горутин)
4. [Что есть в Golang для многопоточности?](Theme/Goroutines.md#многопоточность-go)
5. [Какие есть способы связи между горутинами, какие плюсы и минусы?](Theme/Goroutines.md#способы-связи)
</details>

<details>
<summary><strong>Interface</strong></summary> 📗

1. [Что такое интерфейс?](Theme/Interface.md#интерфейс)
2. [Для чего используются интерфейсы и как они устроены?](Theme/Interface.md#использование-интерфейсов)
3. [Зачем нужен пустой интерфейс?](Theme/Interface.md#пустой-интерфейс)
4. [Чем пустой интерфейс отличается от пустой структуры?](Theme/Interface.md#интерфейс-структура)
5. [Есть интерфейс, а есть указатель на структуру, который nil. Кладем указатель в интерфейс. Что если сравнить интерфейс с nil?](Theme/Interface.md#nil-интерфейс)
</details>

<details>
<summary><strong>MAP</strong></summary> 📗

1. [Что такое мапа?](Theme/MAP.md#мапа)
2. [Как устроена мапа под капотом?](Theme/MAP.md#устройство-мапы)
3. [Какие ключи могут быть у мапы?](Theme/MAP.md#ключи-мапы)
4. [Что произойдет при конкуррентной записи в мапу?](Theme/MAP.md#конкуррентная-запись)
5. [Как работает эвакуация данных?](Theme/MAP.md#эвакуация-данных)
6. [Потокобезопасная ли мапа?](Theme/MAP.md#потокобезопасность)
7. [Map vs Sync.Map](Theme/MAP.md#map-vs-syncmap)
8. [Какая сложность работы с мапой?](Theme/MAP.md#сложность-мапы)
9. [Можно ли взять адрес элемента мапы и почему?](Theme/MAP.md#адрес-элемента)
</details>

<details>
<summary><strong>OOP</strong></summary> 📗

1. [Как устроено ООП в Golang?](Theme/OOP.md#oop-в-go)
2. [Как реализуются принципы (наследование, абстракция, инкапсуляция, полиморфизм)?](Theme/OOP.md#принципы-oop)
3. [Как реализуется наследование в Golang?](Theme/OOP.md#наследование)
</details>

<details>
<summary><strong>Planner</strong></summary> 📗

1. [Как работает планировщик в Golang?](Theme/Planner.md#планировщик)
2. [Как работает вытесняющая многозадачность?](Theme/Planner.md#вытесняющая-многозадачность)
3. [За счет чего достигается параллельное выполнение в Golang?](Theme/Planner.md#параллельное-выполнение)
4. [В чем разница между вытесняющим и кооперативным планировщиком?](Theme/Planner.md#вытесняющий-кооперативный)
5. [Сколько потоков операционной системы мы можем создать?](Theme/Planner.md#потоки-ос)
6. [В планировщике до версии 1.15 какие операции приводят к переключению контекста горутин?](Theme/Planner.md#переключение-контекста)
</details>

<details>
<summary><strong>Primitives of synchronization</strong></summary> 📗

1. [Какие примитивы синхронизации есть в Golang?](Theme/Primitives_of_synchronization.md#примитивы)
2. [Чем мьютекс отличается от семафора?](Theme/Primitives_of_synchronization.md#мьютекс-семафор)
3. [Чем мьютексы отличаются от атомиков?](Theme/Primitives_of_synchronization.md#мьютексы-атомики)
4. [Какие примитивы синхронизации использовал в работе и для чего?](Theme/Primitives_of_synchronization.md#использованные-примитивы)
5. [Как устроена WaitGroup под капотом и как ее можно реализовать самому?](Theme/Primitives_of_synchronization.md#waitgroup)
6. [Когда нужно использовать Mutex, а когда RWMutex?](Theme/Primitives_of_synchronization.md#mutex-rwmutex)
7. [Расскажи про sync.Map](Theme/Primitives_of_synchronization.md#syncmap)
8. [Расскажи про пакет sync](Theme/Primitives_of_synchronization.md#пакет-sync)
9. [Есть общий ресурс. Хотим, чтобы к нему одновременно обращались только N горутин. Как это сделать?](Theme/Primitives_of_synchronization.md#ограничение-горутин)
</details>

<details>
<summary><strong>Race condition</strong></summary> 📗

1. [Что такое race condition?](Theme/Race_condition.md#race-condition)
2. [Как обнаружить race condition?](Theme/Race_condition.md#обнаружение)
3. [Какие есть способы устранения race condition?](Theme/Race_condition.md#устранение)
</details>
