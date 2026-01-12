# Set container

This container can only store **unique** elements.

```cpp
std::set birds = {
    "зяблик"s, "синица"s, "снегирь"s, "зяблик"s, "синица"s
};
// birds = {"зяблик", "синица", "снегирь"}.
```

In `std::set` elements are ordered and stored in red-black tree.
It has standard methods like: add, delete, find but getting an element
can be done only with iterator.

To **initialize**:

```cpp
std::set<int> years;
// OR
std::set years = {1917, 1984};
```

Check if **empty**:

```cpp
std::set words = {
    "world"s, "hello"s, "hello"s
};

words.empty();
```

Check **size**:

```cpp
word.size();
```

To add use **insert**, will only work if unique elements will be added:

```cpp
word.insert(1111);
```

Check if set **contains** element:

```cpp
word.contains(word);
```

**Find** example:

```cpp
auto iter = word.find(word);
```

**Loop** over elements using iteratros and links, can only be const:

```cpp
// Перебор через итераторы.
for (auto iter = words.begin(); iter!= words.end(); ++iter) {
    std::cout << *iter << std::endl;
}

// Перебор через ссылку.
for (const auto& word : words) {
    std::cout << word << std::endl;
}
```

**Editing** is done with `std::erase` and creating new value:

```cpp
// Удаление по значению.
words.erase("hello"s);

// Удаление по итератору.
words.erase(words.find("hello"s));
```

## Sets for complex types

### Sets of vectors

```cpp
std::set<std::vector<int>> set_of_vectors;
    
std::vector<int> vec_1 = {1, 2, 3};
std::vector<int> vec_2 = {4, 5, 6};
std::vector<int> vec_3 = {1, 2, 3};

set_of_vectors.insert(vec_1);
set_of_vectors.insert(vec_2);
set_of_vectors.insert(vec_3);

// set_of_vectors = { [1 2 3], [4 5 6] }.
```

### Sets of classes and structs

**IMPORTANAT**: requirement is comparator for this class or struct.

```cpp
class User {
public:
    User(std::string name, int rd) : name_(name), registration_date_(rd) {}
    
    int GetRegDate() const {
        return registration_date_;
    }
private:
    std::string name_;
    int registration_date_;
};

struct UserComparator {
    bool operator() (const User& u1, const User& u2) const {
        return u1.GetRegDate() < u2.GetRegDate();
    }
};

...

std::set<User, UserComparator> set_of_users{
    User ("Alice"s, 20240907),
    User ("Bobby"s, 20190115),
    User ("Charlie"s, 20220910),
    User ("Mary"s, 20220910),
};
// set_of_users = { 
// ("Bobby", 20190115), 
// ("Charlie", 20220910), 
// ("Alice", 20240907) 
// }
```

## Converting one type of container into another

`std::set` from `std::vector`:

```cpp
std::vector vec{1, 1, 2, 2, 3, 4, 5, 6};

// Создадим множество из элементов вектора.  
auto unique_elements = std::set<int>(vec.begin(), vec.end());

for (auto item : unique_elements) {
    std::cout << item << " "s;
} 
//Вывод: 1 2 3 4 5 6 .
```

`std::vector` from `std::set`:

```cpp
std::set set{'a', 'b', 'c', 'd'};
auto vec = std::vector<char>(set.begin(), set.end());

for (auto item : vec) {
    std::cout << item << " "s;
} 
//Вывод: a b c d .
```

## Create map with different comporator

```cpp
struct Comparator{
    bool operator()(int a, int b) const {
        return std::to_string(a) < std::to_string(b);
    }
};

...
// Словарь со стандартным компаратором.
std::map<int, std::string> accounts {
    {1000000, "Jeff"s}, 
    {5555, "Mary"s},
    {90000, "Jeff"s},
};

// Создадим на его основе новый словарь с Comparator.    
auto new_accounts = std::map<int, std::string, Comparator>(
                             accounts.begin(), accounts.end());

// В первом варианте элементы сортируются по возрастанию числовых ключей:
// 5555, Mary
// 90000, Jeff
// 1000000, Jeff
    
// Во втором варианте ключи сортируются как строки:
// 1000000, Jeff
// 5555, Mary
// 90000, Jeff
```




