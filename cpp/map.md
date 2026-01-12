# Dict consept

Dictionary in standard lib is `std::map`.
`map` realisation is like template.

> Key value pairs have to be same type through map.

> Keys should not repeat.

Special files that store data as dicts: JSON and YAML.
JSON example:

```json
{
    «имя»    : «Иван»,
    «пароль» : «13243546hello»,
    «логин»  : «@FirTheClever»
}
```

**Get** value:

```cpp
auto result = Map["something"s];
```

**Change** value:

```cpp
Map["SomeKey"s] = 1;
```

> Key can not be change, delete -> create with new key.

**Missing** key when called will add key to `map` and do not
cause error if called by operator `[]`:

```cpp
auto result = Map["NewKey"];
```

**Nesting** is available for `map`:

```cpp
auto result = Map["Key"]["SubKey"];
```

## Map types in CPP:

- `std::map` - ordered by *key*, sort itself by key;

- `std::multimap` - ordered by *value*, sort itself by value;

- `std::unordered_map` - unordered, uses hash maps to store *more
effective* than others.

## Set types in CPP:

- `std::set` - stores *unique* elements ordered, no repetitions;

- `std::multiset` - store *NOT unique* elements ordered;

- `std::unordered_set` - unordered, *unique*, *faster* than other `set` types.

> `std::map` search is fast due to **red-black tree**.


**Initialize** map:

```cpp
#include <map> // Подключаем для работы с контейнером.
#include <cstdint>

int main() {
    std::map<std::string, uint64_t> accounts;
}
```

```cpp
// Вариант 1.
std::map<std::string, uint64_t> accounts = {
    {"Jeff"s, 1000000},
    {"Bill"s, 9999999},
};

// Вариант 2.
std::map<std::string, uint64_t> accounts {
    {"Jeff"s, 1000000},
    {"Bill"s, 9999999},
};
```

> If `map` initialize with 2 identical keys, ONLY first one will be added.

**Check if empty** with `empty()`:

```cpp
bool status = accounts.empty();
```

**Size** of map with `size()`:

```cpp
std::size_t size = accounts.size();
```

**For loop** with map:

```cpp
for (auto& [key, value]: accounts) {
    std::cout << key << ", "s << value << std::endl;
    // change value like this:
    value = 0;
}
```

**Send** map to function:

```cpp
void ShowAccounts(const std::map<std::string, uint64_t>& accounts) {
    ...
}
```

OR:

```cpp
ShowAccounts(std::map<std::string, uint64_t>{
    {"Jeff"s, 1000000},
    {"Bill"s, 9999999},
});
```

## Add elements to map

**Insert** elements to map:

```cpp
accounts.insert({"Mark"s, 10101010101});
accounts.insert({"Bill"s, 100});
```

> When new elements added to map whole dict will still be sorted.

> If key already exists it won't be added.

To get **status** of `insert`:

```cpp
auto [iterator, is_inserted] = accounts.insert({"Jeff"s, 100});
```

**Clear** all map:

```cpp
accounts.clear();
```

## Key check

To **check** if key is in map:

```cpp
bool status = account.contains("Key");
```

, but this is **ineffective**, better use **find** that will give iterator of
searched element or return `end()` if not found.

```cpp
auto iterator = accounts.find("Key");
```

## Map comparison

**Overloaded** comparisons:

```cpp
std::map<std::string, uint64_t> old_accounts {
    {"Annie"s, 9859489},
};

std::map<std::string, uint64_t> new_accounts {
    {"Annie"s, 10000},
};

if (old_accounts != new_accounts) {
    std::cout << "Словари не одинаковые"s << std::endl;
}
```

## Swap

**Exchanges** contents of two dicts, very effective:

```cpp
std::map<std::string, uint64_t> old_accounts {
    {"Annie"s, 9859489},
};

std::map<std::string, uint64_t> new_accounts {
    {"Martha"s, 10000},
    {"Mary"s, 1055515},
};

ShowAccounts(old_accounts); // Annie, 9859489.
ShowAccounts(new_accounts); // Martha, 10000, Mary, 1055515.

old_accounts.swap(new_accounts); // Меняем словари местами.

ShowAccounts(old_accounts); // Martha, 10000, Mary, 1055515.
ShowAccounts(new_accounts); // Annie, 9859489.
```

> Trying to use non existant key in map with `[]` will cause
> map to add this key with default value:

```cpp
std::map<std::string, int> prices = {
    {"Milk"s, 99},
    {"Donut"s, 150},
};

// Пытаемся изменить значение элемента, которого ещё нет.
prices["Croissant"s] += 10; 

// Вывод:
// Croissant, 10
// Donut, 150
// Milk, 99
```

> If map is const -> compilation error, can not use `[]` with
> const map.

Create simple **counter**:

```cpp
std::string word = "Hello"s;
std::map<char, int> char_counter;

for (auto c : word) {
    char_counter[c]++; 
}
```

**Call** key , value using `first` and `second` keys from iterator.

> Use `{}` while `insert` to add element, as a pair.

```cpp
// Можно создать пару с помощью {}.
prices.insert({"Salt"s, 40});

// Или использовать переменную.
std::pair<std::string, int> new_item = {"Sugar"s, 80};
prices.insert(new_item);
```

**Moving** iterator using only `++`, `--` and `std::advance`, `std::next`, `std::prev`:

```cpp
// Переход к следующему элементу с помощью инкремента разрешён.
std::cout << (++accounts.begin())->first << std::endl;

// Переход на указанное количество позиций вызовет ошибку.
std::cout << (accounts.begin() + 3)->first << std::endl; // <- Ошибка!
```

**Distance** between iterators with `std::distance`.

**Delete** element from map `std::erase` returns bool about successful delition.
Can also be used with iterators. Can also delete multiple elements with second
argument (not included):

```cpp
auto is_erased = prices.erase("Milk"s);

auto it = prices.find("Milk"s);
prices.erase(it);

prices.erase(it_begin, it_end);
```

**Invalidation** of iterators in map does NOT happen during changes of size
of the map, as elements are not sequentaily stored.


## Complex type maps

**vectors**:

```cpp
std::map<std::string, std::vector<std::string>> recipes {
    {"Carbonara"s, std::vector{"pasta"s, "eggs"s, "cheese"s}},
    {"Cookies"s, std::vector{"flour"s, "sugar"s, "butter"s, "eggs"s}},
};
```

**classes**:

```cpp
class Recipe {
public:
    Recipe(std::string d, std::vector<std::string> i):
        description_(d), ingredients_(i) {}
    // ...

private:
    std::string description_;
    std::vector<std::string> ingredients_;
};

int main() {
    std::map<std::string, Recipe> recipes {
        {
            "Carbonara"s, 
            Recipe{ 
                "Very fast and delicious"s, 
                {"pasta"s, "eggs"s, "cheese"s}
            }
        },
    };
}
```

**maps**

```cpp
// map<название_рецепта, map<ингредиент, количество>>.
std::map<std::string, std::map<std::string, int>> recipes {
    {
        "Carbonara"s, {
            {"Pasta"s, 200},{"Eggs"s, 50},{"Cheese"s, 100},
        }   
    } 
};

std::cout << recipes["Carbonara"s]["Cheese"s] << std::endl;
```

## Comporators

Comporators are set during initialization of map and can not be changed.

```cpp
// Компаратор для сортировки строк по убыванию.
struct Comparator{
    bool operator()(std::string a, std::string b) const {
        return a > b;
    }
};

// Передаём Comparator в виде шаблонного аргумента.
std::map<std::string, int, Comparator> prices = {
    {"Milk"s, 99},
    {"Sugar"s, 110},
    {"Donut"s, 150},
};

// Теперь ключи хранятся в обратном порядке.   
// Sugar: 110
// Milk: 99
// Donut: 150
```

OR shorter with **lambda**:

```cpp
auto comparator = [](std::string a, std::string b) {
    return a > b;
};

std::map<std::string, int, decltype(comparator)> prices = {
    {"Milk"s, 99},
    {"Sugar"s, 110},
    {"Donut"s, 150},
};
```

, `decltype` used because lambda type is not set, other approach is to declare
lambda type:

```cpp
std::function<bool(std::string, std::string)> 
comparator = [](std::string a, std::string b) {
    return a > b;  
};

std::map<std::string, 
         std::string, 
         std::function<bool(std::string, std::string)>
         > prices(comparator);
```

































