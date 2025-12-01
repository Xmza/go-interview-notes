# 🚀 LeetCode Practice (Go Edition)

Здесь собраны популярные задачи для собеседований.
**Правила:**
1. Прочитай условие и примеры.
2. Попробуй решить сам в голове или на листочке.
3. Если застрял — открой "Подсказку".
4. Сравни свое решение с "Решением на Go".

---

## 📋 Оглавление
- [1. Two Sum (🟢 Easy)](#1-two-sum--easy)
- [2. Valid Palindrome (🟢 Easy)](#2-valid-palindrome--easy)
- [3. Contains Duplicate (🟢 Easy)](#3-contains-duplicate--easy)
- [4. Valid Parentheses (🟢 Easy)](#4-valid-parentheses--easy)
- [5. Best Time to Buy and Sell Stock (🟢 Easy)](#5-best-time-to-buy-and-sell-stock--easy)
- [6. Group Anagrams (🟡 Medium)](#6-group-anagrams--medium)
- [7. Maximum Subarray (🟢 Easy / 🟡 Medium)](#7-maximum-subarray--easy--medium)
- [8. Product of Array Except Self (🟡 Medium)](#8-product-of-array-except-self--medium)
- [9. Reverse Linked List (🟢 Easy)](#9-reverse-linked-list--easy)
- [10. Merge Two Sorted Lists (🟢 Easy)](#10-merge-two-sorted-lists--easy)
- [11. Longest Substring Without Repeating Characters (🟡 Medium)](#11-longest-substring-without-repeating-characters--medium)
- [12. 3Sum (🟡 Medium)](#12-3sum--medium)

---

## 1. Two Sum (🟢 Easy)

**Условие:**
Дан массив целых чисел `nums` и целое число `target`.
Верните индексы двух чисел так, чтобы они в сумме давали `target`.
Предполагается, что решение всегда существует и оно одно.

**Пример:**
```text
Input: nums = [2,7,11,15], target = 9
Output: [0,1] (так как 2 + 7 = 9)
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Не используй вложенные циклы, это медленно (O(n²)).
Используй `map` (хеш-таблицу). Пока идешь по массиву, сохраняй в мапу пару `значение -> индекс`.
Для каждого числа проверяй: есть ли в мапе число `target - current`?

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func twoSum(nums []int, target int) []int {
    // Ключ: само число, Значение: его индекс
    m := make(map[int]int)
    
    for i, num := range nums {
        diff := target - num
        // Проверяем, есть ли уже "вторая половинка" в мапе
        if idx, found := m[diff]; found {
            return []int{idx, i}
        }
        // Записываем текущее число
        m[num] = i
    }
    return nil
}
```
**Сложность:** O(n) Time, O(n) Space.

</details>

---

## 2. Valid Palindrome (🟢 Easy)

**Условие:**
Дана строка. Нужно проверить, является ли она палиндромом (читается одинаково слева направо и справа налево).
Учитываются только буквы и цифры, регистр игнорируется.

**Пример:**
```text
Input: "A man, a plan, a canal: Panama"
Output: true
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Используй "Два указателя" (Two Pointers).
Один ставишь в начало (`left`), второй в конец (`right`).
Двигаешь их навстречу друг другу.
Если символ не буква/цифра — пропускай.
Если символы не равны — `false`.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
import (
    "strings"
    "unicode"
)

func isPalindrome(s string) bool {
    // Работаем с рунами для корректной обработки Unicode,
    // хотя для стандартных задач часто хватает байт.
    
    left, right := 0, len(s)-1
    
    for left < right {
        lChar := rune(s[left])
        rChar := rune(s[right])
        
        // Пропускаем не-буквы и не-цифры слева
        if !isAlphanumeric(lChar) {
            left++
            continue
        }
        // Пропускаем не-буквы и не-цифры справа
        if !isAlphanumeric(rChar) {
            right--
            continue
        }
        
        // Сравниваем в нижнем регистре
        if unicode.ToLower(lChar) != unicode.ToLower(rChar) {
            return false
        }
        left++
        right--
    }
    return true
}

func isAlphanumeric(r rune) bool {
    return unicode.IsLetter(r) || unicode.IsDigit(r)
}
```
**Сложность:** O(n) Time, O(1) Space.

</details>

---

## 3. Contains Duplicate (🟢 Easy)

**Условие:**
Дан массив `nums`. Вернуть `true`, если любое значение встречается в массиве хотя бы дважды.

**Пример:**
```text
Input: nums = [1,2,3,1]
Output: true
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

В Go нет встроенного типа `Set` (множество).
Но мы можем использовать `map[int]struct{}` (пустая структура не занимает памяти) как сет.
Если при записи в мапу ключ уже есть — значит дубликат.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func containsDuplicate(nums []int) bool {
    // struct{} занимает 0 байт памяти
    seen := make(map[int]struct{})
    
    for _, num := range nums {
        if _, exists := seen[num]; exists {
            return true
        }
        seen[num] = struct{}{}
    }
    return false
}
```
**Сложность:** O(n) Time, O(n) Space.

</details>

---

## 4. Valid Parentheses (🟢 Easy)

**Условие:**
Дана строка, содержащая скобки `(`, `)`, `{`, `}`, `[`, `]`.
Определить, является ли последовательность скобок правильной.

**Пример:**
```text
Input: "()[]{}" -> true
Input: "(]" -> false
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Классическая задача на **Стек** (Stack).
1. Идешь по строке.
2. Если открывающая скобка — кладешь в стек (append в слайс).
3. Если закрывающая — проверяешь, соответствует ли она последней открытой в стеке. Если да — удаляешь из стека, если нет — ошибка.
4. В конце стек должен быть пуст.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func isValid(s string) bool {
    // Используем слайс как стек
    stack := []rune{}
    // Мапа соответствия закрывающих открывающим
    pairs := map[rune]rune{
        ')': '(',
        ']': '[',
        '}': '{',
    }

    for _, char := range s {
        // Если это закрывающая скобка (есть в ключах мапы)
        if open, ok := pairs[char]; ok {
            // Стек пуст или верхний элемент не совпадает
            if len(stack) == 0 || stack[len(stack)-1] != open {
                return false
            }
            // Pop (удаляем последний элемент)
            stack = stack[:len(stack)-1]
        } else {
            // Это открывающая скобка -> Push
            stack = append(stack, char)
        }
    }
    
    // Если стек пуст — все скобки закрыты верно
    return len(stack) == 0
}
```
**Сложность:** O(n) Time, O(n) Space.

</details>

---

## 5. Best Time to Buy and Sell Stock (🟢 Easy)

**Условие:**
Дан массив цен `prices`, где `prices[i]` — цена акции в `i`-й день.
Нужно найти максимальную прибыль (купить в один день, продать в будущем).

**Пример:**
```text
Input: [7,1,5,3,6,4]
Output: 5 (Купили за 1, продали за 6)
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Не нужен двойной цикл. Пройди массив один раз.
Храни `min_price` (минимальную цену, которую видел) и `max_profit` (максимальную прибыль).
На каждом шаге:
1. Обнови `min_price`, если текущая цена ниже.
2. Иначе, посчитай прибыль (`current - min_price`) и обнови `max_profit`, если она больше.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }
    
    minPrice := prices[0]
    maxProfit := 0
    
    for _, price := range prices {
        if price < minPrice {
            minPrice = price
        } else if (price - minPrice) > maxProfit {
            maxProfit = price - minPrice
        }
    }
    
    return maxProfit
}
```
**Сложность:** O(n) Time, O(1) Space.

</details>

---

## 6. Group Anagrams (🟡 Medium)

**Условие:**
Дан массив строк. Сгруппировать анаграммы вместе.

**Пример:**
```text
Input: ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Анаграммы состоят из одинаковых букв.
Если отсортировать буквы в словах "eat" и "tea", получится одно и то же: "aet".
Используй `map[string][]string`, где ключ — отсортированное слово, а значение — список исходных слов.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
import (
    "sort"
)

func groupAnagrams(strs []string) [][]string {
    groups := make(map[string][]string)
    
    for _, s := range strs {
        // Сортировка букв для создания ключа
        key := sortString(s)
        groups[key] = append(groups[key], s)
    }
    
    result := make([][]string, 0, len(groups))
    for _, v := range groups {
        result = append(result, v)
    }
    return result
}

// Хелпер для сортировки строки
func sortString(s string) string {
    r := []rune(s)
    sort.Slice(r, func(i, j int) bool {
        return r[i] < r[j]
    })
    return string(r)
}
```
**Сложность:** O(n * k * log k).

</details>

---

## 7. Maximum Subarray (🟢 Easy / 🟡 Medium)

**Условие:**
Дан целочисленный массив `nums`. Найти непрерывный подмассив, сумма элементов которого максимальна, и вернуть эту сумму.

**Пример:**
```text
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6 (Подмассив [4,-1,2,1])
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

**Алгоритм Кадейна (Kadane's Algorithm).**
Иди по массиву и на каждом шаге решай:
1. Либо начать новый подмассив с текущего числа.
2. Либо продолжить существующий подмассив (прибавить текущее число к предыдущей сумме).
Выбирай максимум из этих двух вариантов. Параллельно запоминай глобальный максимум.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func maxSubArray(nums []int) int {
    curSum := nums[0]
    maxSum := nums[0]
    
    for i := 1; i < len(nums); i++ {
        // Если предыдущая сумма отрицательная, она нам не нужна, 
        // начинаем новую с текущего элемента.
        // Иначе прибавляем текущий элемент к накопленной сумме.
        if curSum < 0 {
            curSum = nums[i]
        } else {
            curSum += nums[i]
        }
        
        // Обновляем глобальный рекорд
        if curSum > maxSum {
            maxSum = curSum
        }
    }
    return maxSum
}
```
**Сложность:** O(n) Time, O(1) Space.

</details>

---

## 8. Product of Array Except Self (🟡 Medium)

**Условие:**
Дан массив `nums`. Вернуть массив `answer`, где `answer[i]` равно произведению всех элементов `nums`, кроме `nums[i]`.
**Ограничение:** Нельзя использовать операцию деления. Сложность должна быть O(n).

**Пример:**
```text
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Сделай два прохода (или используй два массива префиксов/суффиксов).
1. Сначала пройди слева направо: в `res[i]` запиши произведение всех чисел **до** `i`.
2. Потом иди справа налево: умножай `res[i]` на произведение всех чисел **после** `i`.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func productExceptSelf(nums []int) []int {
    n := len(nums)
    res := make([]int, n)
    
    // 1. Проход слева направо (Prefix Product)
    // res[i] будет содержать произведение чисел слева от i
    prefix := 1
    for i := 0; i < n; i++ {
        res[i] = prefix
        prefix *= nums[i]
    }
    
    // 2. Проход справа налево (Suffix Product)
    // Умножаем накопленное слева на то, что накопили справа
    postfix := 1
    for i := n - 1; i >= 0; i-- {
        res[i] *= postfix
        postfix *= nums[i]
    }
    
    return res
}
```
**Сложность:** O(n) Time, O(1) Space (не считая выходной массив).

</details>

---

## 9. Reverse Linked List (🟢 Easy)

**Условие:**
Развернуть односвязный список.

**Пример:**
```text
Input: 1 -> 2 -> 3 -> 4 -> 5 -> nil
Output: 5 -> 4 -> 3 -> 2 -> 1 -> nil
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Итеративный подход.
Нужны три указателя: `prev` (предыдущий), `curr` (текущий), `next` (следующий).
В цикле: сохраняешь `next`, меняешь стрелку `curr.Next` на `prev`, сдвигаешь `prev` и `curr` вперед.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode // Изначально nil
    curr := head
    
    for curr != nil {
        nextTemp := curr.Next // Запоминаем следующий
        curr.Next = prev      // Разворачиваем стрелку назад
        prev = curr           // Сдвигаем prev
        curr = nextTemp       // Сдвигаем curr
    }
    
    return prev // prev теперь указывает на новую голову
}
```
**Сложность:** O(n) Time, O(1) Space.

</details>

---

## 10. Merge Two Sorted Lists (🟢 Easy)

**Условие:**
Даны головы двух отсортированных списков `list1` и `list2`. Объединить их в один отсортированный список.

**Пример:**
```text
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

Используй технику **Dummy Node** (фиктивный узел).
Создай пустой узел-голову. Заведи указатель `current`.
Сравнивай значения `list1` и `list2`. Меньшее прицепляй к `current.Next` и сдвигай указатель соответствующего списка.
В конце прицепи остаток того списка, который не закончился.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    dummy := &ListNode{} // Фиктивный узел
    tail := dummy
    
    for list1 != nil && list2 != nil {
        if list1.Val < list2.Val {
            tail.Next = list1
            list1 = list1.Next
        } else {
            tail.Next = list2
            list2 = list2.Next
        }
        tail = tail.Next
    }
    
    // Если один список кончился, просто прицепляем второй
    if list1 != nil {
        tail.Next = list1
    } else if list2 != nil {
        tail.Next = list2
    }
    
    return dummy.Next
}
```
**Сложность:** O(n + m) Time, O(1) Space.

</details>

---

## 11. Longest Substring Without Repeating Characters (🟡 Medium)

**Условие:**
Найти длину самой длинной подстроки без повторяющихся символов.

**Пример:**
```text
Input: s = "abcabcbb"
Output: 3 (Подстрока "abc")
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

**Sliding Window (Скользящее окно).**
Используй два указателя (`left`, `right`) и `map` (или массив байт) для отслеживания символов в окне.
Двигай `right` вперед. Если символ уже есть в мапе — сдвигай `left` вправо, пока дубликат не уйдет из окна.
На каждом шаге обновляй максимальную длину.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
func lengthOfLongestSubstring(s string) int {
    // Мапа хранит индекс последнего вхождения символа
    charIndex := make(map[byte]int)
    res := 0
    left := 0
    
    for right := 0; right < len(s); right++ {
        char := s[right]
        
        // Если символ уже встречался и он находится внутри текущего окна
        if idx, found := charIndex[char]; found && idx >= left {
            // Сдвигаем левую границу сразу за дубликат
            left = idx + 1
        }
        
        // Обновляем длину
        if right - left + 1 > res {
            res = right - left + 1
        }
        
        // Запоминаем индекс текущего символа
        charIndex[char] = right
    }
    return res
}
```
**Сложность:** O(n) Time, O(n) Space (для мапы).

</details>

---

## 12. 3Sum (🟡 Medium)

**Условие:**
Дан массив `nums`. Найти все уникальные тройки чисел `[nums[i], nums[j], nums[k]]`, сумма которых равна 0.

**Пример:**
```text
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

<details>
<summary>💡 <b>Подсказка (Алгоритм)</b></summary>

1. Сначала **отсортируй** массив.
2. Проходи циклом по каждому числу (это будет первое число тройки).
3. Для поиска двух оставшихся используй метод **Two Pointers** (как в Two Sum II) на оставшейся части массива.
4. *Важно:* Пропускай дубликаты, чтобы тройки были уникальными.

</details>

<details>
<summary>💻 <b>Решение на Go</b></summary>

```go
import "sort"

func threeSum(nums []int) [][]int {
    sort.Ints(nums) // Сортировка обязательна
    var res [][]int
    
    for i := 0; i < len(nums)-2; i++ {
        // Пропускаем дубликаты для первого числа
        if i > 0 && nums[i] == nums[i-1] {
            continue
        }
        
        // Two Pointers problem
        left, right := i+1, len(nums)-1
        target := -nums[i]
        
        for left < right {
            sum := nums[left] + nums[right]
            if sum == target {
                res = append(res, []int{nums[i], nums[left], nums[right]})
                // Сдвигаем указатели и пропускаем дубликаты
                left++
                right--
                for left < right && nums[left] == nums[left-1] { left++ }
                for left < right && nums[right] == nums[right+1] { right-- }
            } else if sum < target {
                left++
            } else {
                right--
            }
        }
    }
    return res
}
```
**Сложность:** O(n²) Time (из-за вложенного цикла и сортировки).

</details>
