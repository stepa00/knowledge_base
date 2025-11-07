# Lambda

## Create local function

If we need to create three similar function we can initiate them inside
of function namespace to keep the rest of the code tidy.

```cpp
int ChoralSing() {
    // struct inside function
    // it is called local
    struct AnimalSong {
        void operator(){std::string x) const {
            std::cout << x << ", " << x << ", " << x << std::endl;
        };
    };
    
    AnimalSong song;
    song("May");
    song("Gav");
    song("Krja");
}
```

However, there is a better solution **lambda function**.

```cpp
int ChoralSing() {
    // auto - not to thing about the type
    auto song = [](std::string x) {
        std::cout << x << ", " << x << ", " << x << std::endl;
    };

    // ...
}
```

- `[]` - property of lambda function;
- `(std::string x)` - parameters, like normal function;
- `{}` - body of the function.

In _lambda_ functions type of return will be based on `return`:

```cpp
auto make_song_text = [](std::string x) {
    using namespace std::literals;
    return x + ", " + x + ", "s + x;
};

std::string my_text = make_song_text("Mja");
assert(my_text == "Mja, Mja, Mja);
```

**BUT** you can explicitly state type:

```cpp
auto make_song_text = [](std::string x) -> std::string { ...
```

Save *lambda* to an object:

```cpp
auto song = [](std::string x) {
    std::cout << x << ", " << x << ", " << x << std::endl;
};

auto save_song = song;
```

Save *lambda* into *functor*:

```cpp
std::function<void(std::string x)> song = lambda_song;

song("Do-mi-sol"s);
```

To pass variables into *lambda* use: `auto add_part = [&]{...`.

```cpp
auto CreateSinger(const std::string& voice) {
    auto lambda = [/* захват данных */](){
        std::cout << voice << ", " << voice << ", " << voice 
                  << std::endl;
    };

    return lambda;
}
```

Call function the following way:

```cpp
auto meow_singer = CreateSinger("Mja");

meow_singer(); // Mja, Mja, Mja
meow_singer(); // Mja, Mja, Mja
```

We have to implement data capture for returned lambda function.
We can not use `&` capture by link because address of `voice` will be 
destroued right after the call of the function. **SO** we need to call 
by the value `=`!

## Predicat

**Pure functions** - always give the same output for the same input and
doesn't change anything outside itself.

**Predicate** - *pure fuction* that returns *bool*.

Predicate usage example:

```cpp
template<class T, class Predicate>
int CountSome(const std::vector<T>& students,
              const Predicate& what_to_count) {
    int count = 0;
    for (const auto& student: students) {
        if (what_to_count(student)) {
            ++count;
        }
    }
    return count;
}
```

```cpp
int girls = CountSome(students, [](Student t){ 
  return t.gender == Gender::FEMALE;
});
```

### Predicat algorithms

#### count_if

Previous function from standard cpp library.
First, import from `#include <algorithm>` and usage:

```cpp
int girls = std::cout_if(students.begin(), students.end(), [](Student t){
    return t.gender == Gender::FEMALE;
});
```

Another example:

```cpp
// Функция-предикат.
bool IsGirl(Student student) {
    return student.gender == Gender::FEMALE;
}

...
// Использование:
int girls = std::count_if(students.begin(), students.end(), IsGirl);
```

#### cout

Similar to `count_if`, counts elements inside a stucture.


```cpp
// Возьмём определённого студента из вектора.
Student student = students[num];

// Прочитаем контейнер с его оценками.
const std::vector<int>& scores = student.all_scores;

// Используем std::count для подсчёта пятёрок:
int fives_count = std::count(scores.begin(), scores.end(), 5);
```

#### find

Used to look for inclusion of some unit into struct.

```cpp
// Используем std::find для проверки наличия двоек:
/*какой-то тип*/ is_exist = std::find(scores.begin(), scores.end(), 2);
```

```cpp
auto is_exist = std::find(scores.begin(), scores.end(), 2);
if(is_exist == scores.end()) {
  std::cout << "Двоек нет!" << std::endl;
}
```

`find` returns an interator that indicates the location of found
unit and if nothing found sends `scores.end`.

## Callbacks

Callback function is a function that will be called in case of
an event. Callback function will be passed into other function
and executed there.

Callback example:

```cpp
// Функция вызывает колбэк для чётных чисел.
template <typename Callback>
void DoIfEven(const std::vector<int>& numbers, Callback callback) {
    for(auto num : numbers) {
        if (num % 2 == 0) {
            callback(num);
        }
    }
}

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // even_action — это колбэк-функция.
    auto even_action = [](int n){
        std::cout << "Обнаружено чётное число: " << n << std::endl;
    };

    // DoIfEven вызовет наш колбэк для всех чётных чисел.
    DoIfEven(numbers, even_action);
}
```













