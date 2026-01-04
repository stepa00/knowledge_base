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


