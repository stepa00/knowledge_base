# Qt containers

QMap == std::map

QMap methods:

- empty();

- size();

- contains(key);

- find(key) returns iterator.

## insert

In QMap not std::pair but two prameters key and value.

```cpp
std::map<int, std::string> std_map;
std_map.insert( {1, "1"} );

QMap<int, QString> q_map;
q_map.insert(1, "1");
```

In QMap using existing key will overwrite the dictionary.

```cpp
std_map.insert( {1, "2"} );
std::cout << std_map[1] << std::endl; 
// Вывод: 1 — значение осталось прежним.

q_map.insert(1, "2");
std::cout << q_map[1] << std::endl; 
// Вывод: 2 — значение обновилось.
```

## read

QMap returns key / value with methods `key()` / `value()`:

```cpp
for (auto it = std_map.begin(); it != std_map.end(); ++it) {
    std::cout << it->first << " " << it->second << std::endl;
}

for (auto it = q_map.begin(); it != q_map.end(); ++it) {
    // Вариант 1: использование методов key() и value():
    std::cout << it.key() << " " << it.value() << std::endl;
    // Вариант 2 (если ключ не нужен, а достаточно значения). 
    // Разыменование итератора:
    std::cout << *it << std::endl;
}
```

This returns all values:

```cpp
for (const auto& value : q_map) {
    std::cout << value << std::endl;
}
```

This method `keys()` returns all keys and values can be extracted:

```cpp
for (const auto& key : q_map.keys()) {
    // Используем квадратные скобки:
    std::cout << q_map[key] << std::endl;
    
    // Используем value:
    std::cout << q_map.value(key) << std::endl;
}     
```

## erase / remove

With `erase` use iterator, with `remove` use key:

```cpp
// Вариант 1:
q_map.erase(q_map.find(1));

// Вариант 2:
q_map.remove(1);
```

## from QMap to map

```cpp
std::map<int, std::string> std_map = q_map.toStdMap();
```

# QList

QList similar to std::vector. Before working with it better reserve
memory for it to prevent relocation.

## add elements

- push_back

- insert

```cpp
QList<int> list;
// Как в вектор:
list.push_back(2);
list.insert(0, 3);
```

- append

```cpp
QList<int> list{};
// Одно значение:
list.append(1);
// Несколько значений:
list.append({2, 3, 4});
// list: 1, 2, 3, 4.
```

- << 

```cpp
QList<QString> friends_list{};
friends_list << "Маша" << "Вера" << "Илья" << "Пётр";
// friends_list: "Маша", "Вера", "Илья", "Пётр".
```

> In Qt6 QList <=> QVector.

## read and search elements

- []

- at

- back

- front

- find with iterators

- find_if with iterators

```cpp
QList<int>::iterator it = std::find(list.begin(), list.end(), 3);
```

```cpp
auto it = std::find(list.begin(), list.end(), 3);
int index = -1;
if(it != list.end()) {
    index = std::distance(list.begin(), it);
}
```

- indexOf, get index of value

- lastIndexOf, same but in reverse

```cpp
QList<int> list{1, 2, 3, 4, 5};
int index1 = list.indexOf(3);
// Результат: index1 = 2.

int index2 = list.indexOf(42);
// Результат: index2 = -1.
```

```cpp
QList<int> list{1, 2, 3, 4, 5};
int index_2 = list.indexOf(2, 3);
// Ищем значение 2 с индекса 3, то есть в значениях {4, 5}.
// Результат: index_2 = -1.

int index_4 = list.indexOf(4, 3);
// Результат: index_4 = 3.
```

```cpp
QList<int> list{1, 2, 3, 2, 5};
// В этом списке два элемента 2 — на позициях 1 и 3.
int index = list.lastIndexOf(2);
// Результат: index = 3.
// Так как при поиске используется lastIndexOf, то возвращается 3.
```

```cpp
// В этом списке два элемента 2 — на позициях 1 и 3.
QList<int> list{1, 2, 3, 2, 5};

int index_2 = list.lastIndexOf(2, 2);
// Результат: index_2 = 1.
// Поиск выполняется от индекса 2 до индекса 0,
// возвращается индекс 1.

int index_4 = list.lastIndexOf(5, 2);
// Результат: index_4 = -1.
// Так как в подсписке {1, 2, 3} нет значения 5.
```

















