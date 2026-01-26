# Binary search

```cpp
// Возвратим индекс элемента в векторе или nullopt, если не нашли его.
std::optional<size_t> BinarySearch(const std::vector<int>& arr, const int x) {
    size_t a = 0;
    size_t b = arr.size();
    
    // Пока полуинтервал содержит два числа или больше.
    while (b - a >= 2) {
        // Инвариант: если x есть в векторе arr,
        // то он лежит между arr[a] и arr[b - 1].
    
        // Считаем середину полуинтервала.
        const size_t mid = (a + b) / 2;
        
        // Иначе — нужно понять, в какой половине может быть x.
        if (x < arr[mid]) {
            // x в левой половине. Меняем b. 
            // Получим полуинтервал [a, mid).
            b = mid; 
        } else {
            // x в правой половине. Меняем a. 
            // Получим полуинтервал [mid, b).
            a = mid;
        }
    }
    
    // На полуинтервале осталось одно число.
    // Проверим: то ли это, что мы искали.
    if (a == b || arr[a] != x) {
        // Не нашли :(
        return std::nullopt;
    }
    
    return arr[a];
}
```

# Binary search in containers

`std::binary_search` - (start, end, value) -> bool; end element is not
part of the search.

```cpp
std::vector<int> arr = {1, 3, 3, 5, 6, 7};
std::cout << std::binary_search(arr.begin(), arr.end(), 2) << std::endl; // 0
std::cout << std::binary_search(arr.begin(), arr.end(), 3) << std::endl; // 1
std::cout << std::binary_search(arr.begin(), arr.end(), 5) << std::endl; // 1
```

## lower_bound

This method will search for iterator with which the range will end
but this element wont be in it.

`std::lower_bound` - (start, end, value) -> iterator;
if not found -> iterator = end().

```cpp
std::vector<int> arr = {0, 0, 1, 3, 3, 5, 7};

// Итератор на первый элемент не меньше 2.
const auto it1 = std::lower_bound(arr.begin(), arr.end(), 2);
// it1 указывает на 3 с индексом 3.
// it1 ------------↴
// arr = {0, 0, 1, 3, 3, 5, 7}
```

```cpp
// Итератор на первый элемент не меньше 5.
const auto it2 = std::lower_bound(arr.begin(), arr.end(), 5);
// В массиве есть 5. it2 указывает на 5 с индексом 5.
// it2 ------------------↴
// arr = {0, 0, 1, 3, 3, 5, 7}
```

### lower_bound with comparator

```cpp
struct Person {
    std::string name;
    int age;
};

// Вектор, отсортированный по возрасту.
std::vector<Person> persons = ...;

// Вызов lower_bound с произвольным компаратором.
// В данном случае сравнение возраста всегда будет происходить с числом 5.
const auto it = std::lower_bound(
    persons.begin(), persons.end(), 5,
    [](const Person& person, const int age) {
        return person.age < age;
    });

// it — это итератор на первого человека не младше 5 лет.
// Теперь можно, например, узнать, сколько в persons людей младше 5.
std::cout << std::distance(persons.begin(), it) << " людей младше 5 лет"s << std::endl;
```

## upper_bound

```cpp
std::vector<int> arr = {0, 0, 1, 3, 3, 5, 7};

// Итератор на первый элемент больше 2.
const auto it1 = std::upper_bound(arr.begin(), arr.end(), 2);
// it1 указывает на первую тройку в массиве arr с индексом 3.
// it1 ------------↴
// arr = {0, 0, 1, 3, 3, 5, 7}

// Итератор на первый элемент больше 5.
const auto it2 = std::upper_bound(arr.begin(), arr.end(), 5);
// В массиве есть 5, но it2 указывает на 7.
// it2 ---------------------↴
// arr = {0, 0, 1, 3, 3, 5, 7}\
```

## equal_range

returns iterators where value in between.

```cpp
std::vector<int> arr = {1, 2, 3, 3, 4};
auto range3 = std::equal_range(arr.begin(), arr.end(), 3);
// Количество элементов, равных 3.
std::cout << std::distance(range3.first, range3.second) << std::endl; // 2
```

## lower and upper in map and set

They have personal methods, as iterator are not available.

**set**:

```cpp
std::set<int> s = {0, 1, 3, 5, 7};
std::cout << *(s.lower_bound(1)) << ::std::endl; // 1
std::cout << *(s.upper_bound(1)) << ::std::endl; // 3
std::cout << *(s.lower_bound(4)) << ::std::endl; // 5
std::cout << *(s.upper_bound(4)) << ::std::endl; // 5
```

**map**:

```cpp
std::map<int, int> m = {{0, 0}, {1, 0}, {3, 8}, {5, 0}, {7, 9}};
const auto it = m.upper_bound(1);
std::cout << it->first << ": "s << it->second << std::endl; // 3: 8
std::cout << m.lower_bound(7)->second << std::endl; // 9
std::cout << (m.upper_bound(7) == m.end()) << std::endl; // 1
```

> Map and set can not have comporators for these methods, only those
> that were assigned during initializaiton of map and set.


