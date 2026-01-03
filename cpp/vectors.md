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




