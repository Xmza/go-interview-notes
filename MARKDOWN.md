# Markdown Шпаргалка (GitHub Flavored Markdown)

> Все команды и их результат в одном файле.
> **Как использовать:** Копируй код из блоков ниже и вставляй в свой `.md`-файл.

---

## 1. Заголовки

```markdown
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

# H1

## H2

### H3

#### H4

##### H5

###### H6

---

## 2. Текст

### Жирный, курсив, зачёркнутый

```markdown
**Жирный** или __Жирный__  
*Курсив* или _Курсив_  
***Жирный курсив***  
~~Зачёркнутый~~  
```

**Жирный** или **Жирный**  
*Курсив* или *Курсив*  
***Жирный курсив***  
~~Зачёркнутый~~  

### Подчёркивание (HTML)

```markdown
<u>Подчёркнутый</u>
```

<u>Подчёркнутый</u>

### Перенос строки

```markdown
Первая строка (два пробела в конце)  
Вторая строка
```

Первая строка (два пробела в конце)
Вторая строка

---

## 3. Списки

### Нумерованный

```markdown
1. Пункт 1
2. Пункт 2
   1. Вложенный
```

1. Пункт 1
2. Пункт 2

   1. Вложенный

### Маркированный

```markdown
- Пункт A
- Пункт B
  - Вложенный (2 пробела)
```

* Пункт A
* Пункт B

  * Вложенный (2 пробела)

### Чекбоксы (Task list)

```markdown
- [x] Сделано
- [ ] Не сделано
```

* [x] Сделано
* [ ] Не сделано

---

## 4. Ссылки и изображения

### Ссылка

```markdown
[Google](https://google.com)
```

[Google](https://google.com)

### Авто-ссылка

```markdown
https://github.com
```

[https://github.com](https://github.com)

### Изображение

```markdown
![Лого Go](https://go.dev/images/go-logo-blue.svg)
```

![Лого Go](https://via.placeholder.com/150)

### Картинка-ссылка

```markdown
[![Alt](https://via.placeholder.com/100)](https://google.com)
```

[![Alt](https://via.placeholder.com/100)](https://google.com)

---

## 5. Код

### Однострочный

```markdown
Используйте `go mod init`.
```

Используйте `go mod init`.

### Блок кода с языком

````markdown
```go
package main
import "fmt"
func main() {
    fmt.Println("Hello!")
}
```
````

```go
package main
import "fmt"
func main() {
    fmt.Println("Hello!")
}
```

### Блок кода без языка (`pre`)

```markdown
```

Это просто текст
который выводится как есть

```
```

```
Это просто текст
который выводится как есть
```

### Escaping символов

```markdown
\*Эта звёздочка не выделяет* \\
```

\*Эта звёздочка не выделяет\* 

---

## 6. Таблицы

### Базовая таблица

```markdown
| Имя  | Возраст |
|------|---------|
| Иван | 25      |
| Анна | 30      |
```

| Имя  | Возраст |
| ---- | ------- |
| Иван | 25      |
| Анна | 30      |

### Выравнивание

```markdown
| Влево | Центр | Вправо |
|:------|:-----:|------:|
| a     | b     | c     |
| d     | e     | f     |
```

| Влево | Центр | Вправо |
| :---- | :---: | -----: |
| a     |   b   |      c |
| d     |   e   |      f |

### Таблицы с чекбоксами

```markdown
| Задача | Статус |
|--------|--------|
| Сделать | [x]   |
| Проверить | [ ] |
```

| Задача    | Статус |
| --------- | ------ |
| Сделать   | \[x]   |
| Проверить | \[ ]   |

---

## 7. Цитаты

```markdown
> Это цитата
>> Вложенная цитата
```

> Это цитата
>
> > Вложенная цитата

Можно комбинировать со списками:

```markdown
> - Пункт 1
> - Пункт 2
```

> * Пункт 1
> * Пункт 2

---

## 8. Горизонтальная линия

```markdown
---
***
___
```

---

---

---

---

## 9. Эмодзи (GitHub)

```markdown
:smile: :+1: :fire:
```

😄 👍 🔥

---

## 10. Сноски (footnotes)

```markdown
Текст сноски[^1]

[^1]: Это сноска.
```

Текст сноски[^1]

[^1]: Это сноска.

---

## 11. Комментарии (HTML)

```markdown
<!-- Это комментарий, его не видно -->
```

<!-- Это комментарий, его не видно -->

---

## 12. Встраивание HTML

```markdown
<p style="color:red">Красный текст</p>
```

<p style="color:red">Красный текст</p>

---

## 13. Диаграммы (Mermaid)

````markdown
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
````

````
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
````

---

## 14. Детали / спойлер

```markdown
<details>
<summary>Нажми меня</summary>

Скрытый текст внутри блока

</details>
```

<details>
<summary>Нажми меня</summary>

Скрытый текст внутри блока

</details>

---

## 15. Подсветка diff

````markdown
```diff
+ Добавлено
- Удалено
```
````

```diff
+ Добавлено
- Удалено
```

---

## 16. Подсветка JSON/YAML/и др.

````markdown
```json
{
  "key": "value"
}
```
````

```json
{
  "key": "value"
}
```

````markdown
```yaml
name: CI
on: [push]
```
````

```yaml
name: CI
on: [push]
```

---

## 17. Аббревиатуры (HTML)

```markdown
<abbr title="HyperText Markup Language">HTML</abbr>
```

<abbr title="HyperText Markup Language">HTML</abbr>

---

## 18. Автоматическая генерация оглавления (GitHub)

Просто вставь заголовки — GitHub сгенерирует ToC справа.

---

## 19. Вставка видео/iframe (HTML)

```markdown
<iframe width="300" height="200" src="https://www.youtube.com/embed/dQw4w9WgXcQ"></iframe>
```

<iframe width="300" height="200" src="https://www.youtube.com/embed/dQw4w9WgXcQ"></iframe>

---

## 20. HTML-блоки `<pre>`

```markdown
<pre>
Это преформатированный
   текст с пробелами
</pre>
```

<pre>
Это преформатированный
   текст с пробелами
</pre>

---

## 21. Комбинирование Markdown + HTML

```markdown
<blockquote>
  <p><strong>Важное:</strong> Можно использовать HTML внутри Markdown</p>
</blockquote>
```

<blockquote>
  <p><strong>Важное:</strong> Можно использовать HTML внутри Markdown</p>
</blockquote>

## 22. Теги 

> [!NOTE]
> Полезная информация.

> [!TIP]
> Совет.

> [!IMPORTANT]
> Важно знать.

> [!WARNING]
> Предупреждение.

> [!CAUTION]
> Опасность.



[!NOTE]Полезная информация.


[!TIP]Совет.


[!IMPORTANT]Важно знать.


[!WARNING]Предупреждение.


[!CAUTION]Опасность.


## 23. Диаграммы (Mermaid)
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```


graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;


## 24. Медиа






  




  


[Файл в репо](/path/to/file.md)


Файл в репо
