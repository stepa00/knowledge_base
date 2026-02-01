# File system in cpp

Use class `std::filesystem::path`.

```cpp
#include <filesystem>
#include <iostream>

using namespace std;

int main() {
    using filesystem::path;

    path p = path("a") / path("folder") / path("and") / path("..") / path("a") / path("file.txt");

    // выводим естественное представление пути в std::string
    cout << p.string() << endl;
}
// result: a/folder/and/../a/file.txt
```

Use literal overload to make path writing smaller:

```cpp
path operator""_p(const char* data, std::size_t sz) {
    return path(data, data + sz);
}

"a"_p / "folder"_p / "and"_p / ".."_p / "a"_p / "file.txt"_p
```

Get absolute path:

```cpp
path p = "a"_p / "folder"_p / "and"_p / ".."_p / "a"_p / "file.txt"_p;

cout << "Исходный вид: "s << p.string() << endl;
p = filesystem::absolute(p);
```

## Creating directories

`create_directory` and `create_directories`

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>

using namespace std;
using filesystem::path;

path operator""_p(const char* data, std::size_t sz) {
    return path(data, data + sz);
}

void CreateFile(path p) {
    ofstream file(p);
    if (file) {
        cout << "Файл создан: "s << p.string() << endl;
    } else {
        cout << "Ошибка создания файла: "s << p.string() << endl;
    }
}

int main() {
    error_code err;

    path p = "tmp"_p / "a"_p / "folder"_p;

    CreateFile(p / "file.txt"_p);

    filesystem::create_directory(p, err);
    if (err) {
        cout << "Ошибка создания папки через create_directory: "s << err.message() << endl;
    } else {
        cout << "Успешно создана папка через create_directory: "s << p.string() << endl;
    }

    filesystem::create_directories(p, err);
    if (err) {
        cout << "Ошибка создания папки через create_directories: "s << err.message() << endl;
    } else {
        cout << "Успешно создана папка через create_directories: "s << p.string() << endl;
    }

    CreateFile(p / "file.txt"_p);

    path p2 = p.parent_path() / "folder2"_p;
    filesystem::create_directory(p2, err);
    if (err) {
        cout << "Ошибка создания папки через create_directory: "s << err.message() << endl;
    } else {
        cout << "Успешно создана папка через create_directory: "s << p2.string() << endl;
    }

    CreateFile(p2 / "file.txt"_p);
}
```

## Check object type

How to check it is a file or directory.

```cpp
void PrintFileOrFolder(filesystem::path p) {
    error_code err;
    auto status = filesystem::status(p, err);
    
    if (err) {
        return;
    }

    if (status.type() == filesystem::file_type::regular) {
        cout << "Путь "s << p.string() << " указывает на файл"s << endl;
    } else if (status.type() == filesystem::file_type::directory) {
        cout << "Путь "s << p.string() << " указывает на папку"s << endl;
    } else {
        cout << "Путь "s << p.string() << " указывает на другой объект"s << endl;
    }
}
```

## Iterate over directories

```cpp
#include <filesystem>
#include <fstream>
#include <iostream>

using namespace std;
using filesystem::path;

path operator""_p(const char* data, std::size_t sz) {
    return path(data, data + sz);
}

...

int main() {
    path p = "a"_p / "folder"_p;
    filesystem::create_directories(p);
    filesystem::create_directory(p / "subfolder"_p);
    std::ofstream(p / "file.txt"_p) << "File content"s;

    for (const auto& dir_entry : filesystem::directory_iterator(p)) {
        PrintFileOrFolder(dir_entry.path());
    }
}
```





