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













