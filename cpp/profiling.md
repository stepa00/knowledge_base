# Profiling

Will be using `<chrono>` library for profiling.

```cpp
#include <chrono>
#include <iostream>
#include <thread>

using namespace std;

int main() {
    cout << "Ожидание 5s..."s << endl;
    const chrono::steady_clock::time_point start_time = chrono::steady_clock::now();

    // операция - ожидание 5 секунд
    this_thread::sleep_for(chrono::seconds(5));
    const chrono::steady_clock::time_point end_time = chrono::steady_clock::now();

    const chrono::steady_clock::duration dur = end_time - start_time;
    cout << "Ожидание завершено"s << endl;
}
```

```cpp
#include <chrono>
#include <iostream>
#include <thread>

using namespace std;
using namespace chrono;

int main() {
    cout << "Ожидание 5s..."s << endl;
    const auto start_time = steady_clock::now();

    // операция - ожидание 5 секунд
    this_thread::sleep_for(seconds(5));
    const auto end_time = steady_clock::now();

    const auto dur = end_time - start_time;
    cerr << "Продолжительность сна: "s << duration_cast<milliseconds>(dur).count() << " ms"s << endl;

    cout << "Ожидание завершено"s << endl;
}
```

To send the output of `cerr` stream use the following command:

```bash
./<file_name> 2>err.txt
```

Add `std::literals` to code:

```cpp
#include <chrono>
#include <iostream>
#include <thread>

using namespace std;
using namespace chrono;
// хотите немного магии? тогда используйте namespace literals
using namespace literals;

int main() {
    cout << "Ожидание 5s..."s << endl;
    const auto start_time = steady_clock::now();

    // операция - ожидание 5 секунд
    this_thread::sleep_for(5s);
    const auto end_time = steady_clock::now();

    const auto dur = end_time - start_time;
    cerr << "Продолжительность сна: "s << duration_cast<chrono::milliseconds>(dur).count() << " ms"s << endl;

    cout << "Ожидание завершено"s << endl;
}
```

Examples of template class `chrono::duration`:

```cpp
chrono::duration<int64_t>; // seconds
chrono::duration<int64_t, chrono::milli>; // milliseconds
chrono::duration<int64_t, ratio<60>>; // minutes
```

## Log with custructor and distructor

```cpp
class LogDuration {
public:
    LogDuration() {
    }

    ~LogDuration() {
        const auto end_time = steady_clock::now();
        const auto dur = end_time - start_time_;
        cerr << duration_cast<milliseconds>(dur).count() << " ms"s << endl;
    }

private:
    // В переменной будет время конструирования объекта LogDuration
    const steady_clock::time_point start_time_ = steady_clock::now();
};

int main() {
    cout << "Ожидание 5s..."s << endl;

    {
        LogDuration sleep_guard;
        // операция - ожидание 5 секунд
        this_thread::sleep_for(5s);
    }

    cout << "Ожидание завершено"s << endl;
}
```

## Macros

```cpp
#define PROFILE_CONCAT_INTERNAL(X, Y) X ## Y
#define PROFILE_CONCAT(X, Y) PROFILE_CONCAT_INTERNAL(X, Y)
#define UNIQUE_VAR_NAME_PROFILE PROFILE_CONCAT(profileGuard, __LINE__)
#define LOG_DURATION(x) LogDuration UNIQUE_VAR_NAME_PROFILE(x)

int main() {
    int UNIQUE_VAR_NAME_PROFILE;
}
```

, result:

```cpp
#line 1 "main.cpp"

int main() {
    int profile_guard_6;
}
```
