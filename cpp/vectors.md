# Vectors

## Concept Model-View-Controller (MVC)

Pattern - scheme of classes and their interactions. MVC is a pattern.

MVC has **three** parts:

1. Model - data, parameters and constraints;

2. View - user interface;

3. Controller - controller is a bridge between user and model.

## Vector basics

1. zero elements:

```cpp
#include <vector>

std::vector<int> numbers{};
```

2. vector with same elements:

```cpp
std::vector<init> numbers(10);
// Output: 0, 0, 0, ...
```

```cpp
std::vector<std::string> numbers(10, "tik-tak");
// Output: "tik-tak", "tik-tak" , ...
```

3. vector of list of elements:

```cpp
std::vector<int> numbers = {1, 2, 3};

std::vector<int> another_numbers({10, 11, 12});
```

4. vector without type:

```cpp
std::vector numbers = {1, 2, 3};

std::vector text = {"ab", "acb", "abcd"};
// thouse wont be strings but <conts char*>
```

5. vector through another vector

```cpp
std::vector<int> numbers = {1, 2, 3};

std::vector another_numbers = numbers;
```

6. vector from range (see futher)

...

## Vector methods

 - `vector.push_back("str");` - add to the end;

 - `vector.pop_back();` - remove from the end, can not use this method
 on empty vector, check for empty first:
 
 ```cpp
if(!queue.empty()) {
    // vector is not empty
}
```

- `size()` - to check for size of vector;

- `vector.clear()` - to clear the vector.

### Get values from vector

Get element of vector by index use:

- `[]` - will return unexpected behaviour if searching outside of vector size;

- `at()` - will return error if searching outside of vector size.

Get first and last elements:

- `front()` - first;

- `back()` - last.

> Methods working with exact element need extra checks, as these
> elements should be present to prevent unexpected behaviour.

## Iterators

```cpp
std::vector<std::string>::iterator some_iter = client_name.begin();
// too long

auto auto_iter = client_names.begin();
// shorter -> better
```

, `begin()` returns fist element of the vector.

### Work with referenced element by iterator

To **read** the element from iterator use **dereferensing**:

```cpp
std::string first_client_name = *some_iter;

std::count << *some_iter << std::endl;

PayRoyalty(*some_iter);
```

To **change** the element using iterator:

```cpp
*some_iter = "Elon";
```

To **read** field and **call** method by iterator:

```cpp
std::vector<Student> students = ...;

auto student_iter = students.begin();

std::cout << (*student_iter).GetAvgScore() << std::endl;
```

, but this is long and unclear, so:

```cpp
std::cout << student_iter->GetAvgScore() << std::endl;
```

To **delete** element by iterator:

```cpp
client_names.erase(some_iter);

// will remove first element from vector if begin() used
```

> `*iter` means that it points to a certain element in vector

### Move iterator pointer

To **move** iterator pointer use `++` and `--`:

```cpp
std::vector<std::string> client_names = {...};

auto some_iter = client_names.begin();
// points to the first element

some_iter++;
// moved pointer to the second element

++some_iter;
// moves pointer to the third element
// works same as in numbers

some_iter--;
// moves pointer back to the second
```

> Some container can be bidirectional while others are unidirectional

Iterator can not be moved to the **left** of the first element. BUT
can be moved to the **right** of the last element. After last
element you **can not** dereference but you can move back to the left.

Operator `end()` will move to the fake element to the **right** of
the last element.

If vector is empty and `begin()` is called iterator will point to the
fake element and `begin()` == `end()`.

Make **for** loop with iterators:

```cpp
std::vector<std::string> names = {...};

for (auto iter = names.begin(); iter != names.end(); ++iter) {
    // some code
}
```

**Jump over** few elements:

```cpp
auto iter = numbers.begin();

iter +=3;
// points to the index 3

auto iter2 = iter - 2;
// pointer moved 2 positions back
```

**Other** moving methods:

```cpp
std::advance(iter, 2);
// move to the right

std::iter2 = std::next(iter); // iter2 = iter + 1
std::iter3 = std::prev(iter); // iter3 = iter - 1
```

Select element by **index**:

```cpp
auto iter1 = numbers.begin() + 0; // index 0

auto iter1 = numbers.begin() + 4; // index 4 
```

**Check** index correctness:

```cpp
if (number >= 0 && number < numbers.size()) {
    auto some_iter = numbers.begin() + number;
}
```

**Find** distance between iterators:

```cpp
int distance = iter2 - iter1;
// iter2 = 3, iter1 = 1;
// int resutl = 2

// same result
int distance = std::distance(numbers.begin(), numbers.end());
```

### Const iterators

Constant iterators  do not allow to change elements of the container.

```cpp
std::vector<T>::const_iterator iter = client_names.begin();

*iter = "Elon"; // Error!

auto iter2 = client_names.cbegin();
auto iter3 = client_names.cend();
```
 
> If you use a const vector, iterators will also be const:

```cpp
void ProcessClients(const std::vector<Client>& clients) {
    // iter = const
    auto iter = clients.begin();
    ...
```

### Reverce iterators

Same as normal iterator but go in reverse:

```cpp
std::vector<std::string> names = {...};

for (auto iter = names.rbegin(); iter != namess.rend(); ++iter) {
    ...
}
```

`rend()` - is one before the first element of vector.

### Other usefull functions

**For** loop simplified:

```cpp
std::vector<std::string> names = {...};
for (const auto& value : names) {
    ...
}
```

**Insert** / **Erase** adds / removes element before the element that iterator points to:

```cpp
auto iter = ++names.begin();

names.insert(iter, "Elon");
// places "Elon" after the zero position of vector

auto iter2 = names.insert(iter, "Jeff");
// iter2 now points to a new index = 1 position 

auto iter3 = names.erase(iter);
// iter3 points to index = 1 position
```

**Remove** is more effective `std::remove_if`.

### Invalidation of iterators

Every time methods that change the size of the container are used - iterator become
**invalid** and can not be used, as now they cause undefinded behaviour. This
happens because container when changed can be moved to a new place in memory, where it
fits and pointers of iterators are not refreshed and point to an old place.

Correct way to use **for** loop with changes to container:

```cpp
for (auto iter = some_vec.cbegin(); iter != some_vec.cend() ++iter) {
     iter = some_vec.insert(iter, *iter);
     iter++;
}
```

Using a **range-based** loop use another container.

## Vector use cases

Vector stores all its elements in **heap** memory.
Vector does not have methods to add and remove first element, as
those operations are not effective.
Vector has a dedicated space for its elements and when more space needed
vector goes to another part of the memory taking all existing elements with
it. So when adding new element to vector when it is full can be long and
difficult.
To find out how much space vector has there is a method `capacity()`.

```cpp
v.size() // size - 4
v.capacity() // capacity - 6
v.data() // address of the first element
```

, when we add 3 elements, vector will move to another place and capacity
will be 12, while size 7. This is **relocation** and that is when
**invalidation** of iterators happen.

`resize()` can change capacity of the vector in both ways, creates new
elements in accordance will entered value.

`reserve()` will take the amount of space needed for vector. Allocates space
without creating new elements in these spaces.

Use case of `reserve()`:

```cpp
result.reserve(students.size());

for (const auto& s : students) {
    result.push_back(s.name);
}

return result;
```

Vector can not store poiters and functions.

## Algorithms, iterators and vectors

Using iterators we can achive new results with `find()` and `find_if()`.

```cpp
std::vector<int> nums{1, 2, 3, 5, 2, 3, 5, 7, 1};
auto it = nums.begin();
do {
  it = std::find(it, nums.end(), 5);
  if(it != nums.end()) {
      std::cout << std::distance(nums.begin(), it) << std::endl;
      it++;
  }
} while(it != nums.end());
// Вывод: 3 и 6.
```

### Algorithm `std::accumulate`

```cpp
std::vector<int> first_programer = {100, 210, 134, 89, 256};
int sum_first_programer = std::accumulate(
    first_programer.begin(), // Начало диапазона.
    first_programer.end(),   // Конец диапазона.
    0                        // Начальное значение.
);
```

, will sum up all the values in the vector. It can also work
with chars.

```cpp
std::vector<char> source_line = 
    {'A', 'l', 'g', 'o', 'r', 'i', 't', 'h', 'm', 's'};
// result = {'A', 'l', 'g', 'o', 'r', 'i', 't', 'h', 'm', 's'}
```

It also works with strings:

```cpp
std::vector<std::string> v_str{"Hello", ", ", "Algorithms", "!"};
std::string ss = std::accumulate(v_str.begin(), v_str.end(), std::string{});
std::cout << ss.c_str() << std::endl;
// Будет выведено: Hello, Algorithms!
```

Can work with custom **structs**, just specify the operation of
summation first:

```cpp
// Определим структуру с двумя числами - int и double.
struct TwoNums {
    int int_num;
    double double_num;
};

TwoNums operator+(const TwoNums& lhs, const TwoNums& rhs) {
    return {lhs.int_num + rhs.int_num, lhs.double_num + rhs.double_num};
}

std::vector<TwoNums> v_structs{{1, 1.2}, {2, 1.2}, {3, 1.2}, {4, 1.2}};

TwoNums  nums = std::accumulate(v_structs.begin(), v_structs.end(), TwoNums {});
```

## Min / max

`std::min`, `std::max` and `std::minmax` operators and use case:

```cpp
int max_int = std::max(4, 5);  // 5.
int min_int = std::min(-7, 3); // -7.
auto [min, max] = std::minmax(14, 6);  // min = 6, max = 14.
auto [min2, max2] = std::minmax(
    {-0.4, 1e+5, 2.});  // min2 = -0.4, max2 = 1e+5.
```

`std::min_element`, `std::max_element` and `std::minmax_element` operators
and use case:

```cpp
// Позиции:              0  1  2  3  4  5  6  7
std::vector<int> nums = {1, 2, 5, 6, 7, 3, 7, 1};
auto it = std::max_element(nums.begin(), nums.end());
std::cout << "Максимум на позиции " << (it - nums.begin()) 
          << ", значение: " << (*it) << std::endl;
// Напечатает:
// Максимум на позиции 4, значение: 7.
```

To compare structs min/max operators require comparison operators `<>=`:

```cpp
bool operator<(const TwoNums& lhs, const TwoNums& rhs) {
    auto to_tuple = [](const TwoNums& val){
        return std::tie(val.int_num, val.double_num);
    };
    
    // Сравним структуры как кортежи.
    return to_tuple(lhs) < to_tuple(rhs);
}
```

### Lambdas

`min`, `max` and `minmax` and others use `std::less` but other comparators
can be used like `std::abs`:

```cpp
std::vector<int> vec_int = {-5, 3, -9, -1, 6};

// Максимум по абсолютному значению:
auto it = std::max_element(vec_int.begin(), vec_int.end(), [](int a, int b) {
    return std::abs(a) < std::abs(b); 
});

// Выведет текст - "Максимальный элемент по модулю: -9".
std::cout << "Максимальный элемент по модулю: " << *it << std::endl;
```

## Coparing

Vectors can be compared lexicographically:

```cpp
std::vector<int> v1 = {1, 2, 3};
std::vector<int> v2 = {3, 2, 1};

std::cout << v1 > v2 << std::endl;
// result: false
```

, another example:

```cpp
std::vector<std::string> v1{"I", "love", "C++"};
std::vector<std::string> v2{"I", "love", "C++", "very", "much"};

std::cout << std::boolalpha;

// Вектор v1 – префикс v2 и поэтому v1 меньше.
std::cout << "v1 < v2: " << (v1 < v2) << std::endl; // true.
```

### Coparing with iterators

- `std::equal`

- `std::lexicographical_compare`

- `std::lexicographical_compare_three_way`

#### std::equal

Check if word if written backwords the same way, as forwards:

```cpp
std::string s;
std::cin >> s;

// Такой алгоритм не будет работать для русского языка,
// но проблему решит QString.
if (std::equal(s.begin(), s.end(), s.rbegin(), s.rend())) {
    std::cout << "Строка " << s << " - палиндром!" << std::endl;
} else {
    std::cout << "Увы, строка " << s << " не палиндром" << std::endl;
}
```

Check if vector is a prefix of another vector:

```cpp
template<class T>
bool CheckPrefix(const std::vector<T>& prefix, const std::vector<T>& full) {
    // Префикс не может быть длиннее всей строки.
    if (prefix.size() > full.size()) {
        return false;
    }
    
    // Сравниваем вектор prefix с началом вектора full
    // длины prefix.size().
    return std::equal(prefix.begin(), prefix.end(), 
          full.begin(), full.begin() + prefix.size());
}
```

### std::sort

Use case sorting sailing teams (by less points):

```cpp
struct Team {
    // overload comparison
    auto operator<=>(const Team& other) const {
        return points <=> other.points;
    }
    
    std::string name;
    int points;
};

// using standart comparison std::less, no extra coparator needed
std::vector<Team> teams;
teams.push_back({.name = "Team1", .points = 5});
teams.push_back({.name = "Team2", .points = 2});
teams.push_back({.name = "Team3", .points = 26});
teams.push_back({.name = "Team4", .points = 18});

std::sort(teams.begin(), teams.end());
```

To reverse the list of winners use `std::reverse`:

```cpp
std::reverse(teams.begin(), teams.end());
```

It is prefered to use correct comporator from the beginning
instead of using reverse after sort.

## Remove selected elements from vector

Task to remove all dublicates from the vector:

```cpp
std::vector<int> GetUnique(const std::vector<int>& src) {
    std::vector<int> result;
    for (auto i : src) {
        // Добавляем элемент, если его ещё нет в result.
        if (std::find(result.begin(), result.end(), i) == result.end()) {
            result.push_back(i);
        }
    }
    return result;
}
```

, this one is **slow**. Another option, first sort vector:

### std::unique

```cpp
std::vector<int> GetUniqueOfSorted(const std::vector<int>& src) {
    if (src.empty()) {
        return {};
    }
    
    std::vector<int> result;
    result.push_back(src.front());
    for (auto iter = src.begin() + 1; iter != src.end(), ++iter) {
        // Если элемент не равен предыдущему добавим его.
        if (*iter != *(iter - 1)) {
            result.push_back(*iter);
        }
    }
    return result;
}
```

Another standard option is `std::unique` than moves dublicats to the end
of the vector, where they can be removed. Example:

```cpp
auto to_del = std::unique(some_vec.begin(), some_vec.end());
// to_del iterator point to the first of dublicates at the end of the
// vector, where next it is easily erased
some_vec.erase(to_del, some_vec.end())
```

### std::remove / remove_if

Those algorithms move selected elements to the end of the vector and
return iterator pointing to them. Example:

```cpp
std::vector<int> vec = {4, 2, 6, 3, 1, 5, 3, 2, 15, 0, 3, 1};

// Удалим двойки.
auto to_del = std::remove(vec.begin(), vec.end(), 2);
vec.erase(to_del, vec.end());
// vec == {4, 6, 3, 1, 5, 3, 15, 0, 3, 1};

// Удалим нечётные числа.
auto to_del2 = std::remove_if(vec.begin(), vec.end(), [](int i){
    return i % 2 != 0;
});
vec.erase(to_del2, vec.end());
// vec == {4, 6, 0};
```

## Generating random

Example of pseudo random vector:

```cpp
// Создадим генератор.
std::mt19937 gen;

// Нам нужны числа от 1 до 1 000 000.
std::uniform_int_distribution<> dist(1, 1'000'000);

// Создаём вектор для хранения случайных чисел из пяти элементов.
std::vector<int> random_numbers(5);

// И заполним его случайными числами.
for (auto& num : random_numbers) {
    num = dist(gen);
}
```

, this wil always generate the same sequence, to change it - 
change the seed:

```cpp
// Используем сид 1:
std::mt19937 gen(1);

std::uniform_int_distribution<> dist(1, 1'000'000);
std::vector<int> random_numbers(5);
for (auto& num : random_numbers) {
    num = dist(gen);
}

// Теперь в векторе будут числа 417022, 997185, 720325, 932558, 115.
```

, but *seed* is not random, add system randomize, which is
slow and not recomended for generation of vector number but
good anough for seed generation:

```cpp
// Аппаратный генератор случайного значения:
std::random_device rd;   

// Инициализация начальным значением, полученным от аппаратного генератора:
std::mt19937 gen(rd());
```

### Shuffle vector elements

`std::shuffle`

```cpp
// Заполним вектор значениями.
std::vector<int> numbers{1, 2, 3, 4, 5, 6};

// Создание генератора со случайным начальным значением.
std::random_device rd;  
std::mt19937 gen_shuffle(rd());

// Перемешаем элементы в векторе.
std::shuffle(numbers.begin(), numbers.end(), gen_shuffle);
```

, result will be `numbers` vector with different arrangement of
elements.

## Filling up vectors

```cpp
// Десять единиц:
std::vector<int> v_nums_one(10, 1);

// Вектор из ста фраз "Do it, just do it!":
std::vector<std::string> just_do_it(100, "Do it, just do it!");

// Вектор из 42-х других векторов:
std::vector<std::vector<int>> one_two_three(42, std::vector{1, 2, 3});
```

Show case of a meandr of 48000 count per second and frequency of 240 Hz:

```cpp
// Создадим вектор из 48 000 нулей.
std::vector<int16_t> signal(48000, 0);

int pos = 0;
int16_t current_elem = 10000;

for(auto& elem: signal) {
    // Если дошли до 100-го, меняем знак и сбрасываем счётчик.
    if (pos++ == 100) {
        pos = 0;
        current_elem = -current_elem;
    }
    
    // Меняем элемент вектора.
    elem = current_elem;
}

// OR 

for(auto& elem: signal) {
    elem = ((pos++) % 200 < 100) ? current_elem : -current_elem;
}
```

### std::fill

Fills vector according to repeated logic. Between two iterators fills
vector with third value.

```cpp
// Конструктор создаст вектор из 20 букв a:
vector<string> repeat(20, "a");

// Напечатаем 20 a:
PrintVector(repeat);

// Заполним вектор буквами b и напечатаем 20 букв b:
std::fill(repeat.begin(), repeat.end(), "b");
PrintVector(repeat);

// Заполним начало буквами c и напечатаем 10 букв c и 10 букв b:
std::fill(repeat.begin(), repeat.begin() + 10, "c");
PrintVector(repeat);
```

Back to meander:

```cpp
std::vector<int> meander(48000, 0);

// Заполним первые 200 отсчётов – одиночный меандр.
std::fill(meander.begin(), meander.begin() + 100, 10000);
std::fill(meander.begin() + 100, meander.begin() + 200, -10000);
```

> If vector of size 10 fill with size 100 that will create undefined
> behaviour. `std::fill` can not create new elements and change size
> of a vector.

`std::fill_n` - instead of second iterator takes number of elements.

```cpp
std::fill_n(meander.begin(), 100, 10000);
```

### std::copy_n

Accepts iterator to start, number of elements and where to copy.

```cpp
const int wave_length = 200;
for (size_t offset = wave_length;
    offset < signal.size(); 
    offset += wave_length) {
    
    // Копируем первые wave_length элементов в позицию offset.
    std::copy_n(meander.begin(), wave_length, signal.begin() + offset);
}
```

, there is an issue with this one, destination smaller than copied unit.
Safer version:

```cpp
 for (size_t offset = wave_length;
      offset < signal.size();
      // Не делаем инкремент здесь.
     ) {
     
     // Определим, сколько максимально можем скопировать.
     int max_copy = signal.size() - offset;
     
     // Сколько нужно скопировать: wave_length элементов,
     // но не больше max_copy.
     auto to_copy = std::min(wave_length, max_copy);
     
     // Теперь копируем не wave_length, а to_copy.
     std::copy_n(meander.begin(), to_copy, signal.begin() + offset);

     // Сделаем инкремент здесь.
     offset += to_copy;
 }
```

### std::copy

Takes:

1. start iterator
2. end iterator
3. place to copy to

```cpp
assert(the_best.size() <= description.size()); // Иначе неопределённое поведение.
std::copy(the_best.begin(), the_best.end(), description.begin());
```

### std::back_inserter

`std::back_inserter` creates a special output iterator that appends values to the end
of a container instead of writing into existing elements. When an algorithm assigns a 
value to this iterator, it internally calls `container.push_back(value)`.

```cpp
//  Создадим вектор без указания количества элементов:
std::vector<int16_t> signal;

// Добавим 100 чисел 10000 в signal.
std::fill_n(std::back_inserter(signal), 100, 10000);
// Добавим ещё 100 чисел -10000.
std::fill_n(std::back_inserter(signal), 100, -10000);

std::cout << signal.size();
// Результат вывода на печать: 200.
```

```cpp

```

, for this function to work we need to add this part before:

```cpp
//  Создадим вектор без указания количества элементов:
std::vector<int> signal;
signal.reserve(48000);

// Добавим 100 чисел 10000 в signal.
...
```

## Functional style

### std::generate_n

`std::generate_n` - Call this function N times and write each returned value 
into the container, starting from here. `std::generate_n(output_it, count, generator);`


```cpp
std::vector<int16_t> signal;
signal.reserve(48000);

const double amplitude = 15000;
const int wave_length = 200;

// Используем mutable-лямбда функцию. Она может менять захваченные значения.
// В данном случае меняем i, который инициализируем в 
// квадратных скобках: i = 0.
auto generator = [=, i = 0]() mutable {
    // Возвратим значение. Алгоритм generate_n добавит его
    // в вектор.
    return amplitude * sin(2 * i++ * std::numbers::pi / wave_length);
};

std::generate_n(std::back_inserter(signal), wave_length, generator);
```

### std::transform

Reads elements from one range -> changes it -> writes into another range.

```cpp
void ApplyDecay(std::vector<int16_t>& signal, size_t max_length) {
    size_t decay_length = std::min(signal.size(), max_length);

    // [=] – captures decay_length by value
    // i = decay_length – initializes a private counter
    // mutable – allows i to be modified on each call

    auto transformer = [=, i = decay_length](int16_t source) mutable {
        double factor = double(i--) / decay_length;
        return static_cast<int16_t>(source * factor);
    };

    auto start_pos = signal.end() - decay_length;
    std::transform(start_pos, signal.end(), start_pos, transformer);
}
// initial: [ 1000, 1000, 1000, 1000, 1000 ]
// result: [ 1000,  800,  600,  400,   0 ]
```













