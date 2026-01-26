# Files in cpp

To work with files use `ifstream`, `ofstream` and `fstream`.
Use library `<fstream>`.

```cpp
#include <fstream>
#include <iostream>
#include <string>

using namespace std;

int main() {
    {
        ifstream in_file("test.txt"s);
        int x; 
        if (in_file >> x) {
            cout << "Из файла прочитано число "s << x << endl;
        }
    }

    {
        ofstream out_file("test.txt"s);
        out_file << 100 << 500 << endl;
    }
}
```

## Stream errors

- `badbit` - critical error;

- `failbit` - non critical error;

- `eofbit` - reached the end of file, last read could also be
unsuccessful.

If `failbit` or `badbit` is true, stream wont function untill reset.
Use `clear` to clear error flags.

Use `input.good()` to prevent the last cycle of the loop, as when
input is finished non of the error bits is set yet but method can catch
`eorfbit`.

## fstream

A more flexible version of `ofstream` and `ifstream`.
Special flags to set the parameters of `fstream` behaviour:

- `ios::in` - allow read;

- `ios::out` - allow write;

- `ios::app` - append to file content;

- `ios::ate` - first write at the end of file;

- `ios::trunc` - erase content when opened;

- `ios::binary` - binary mode (in the future lectures).

Modes can be combined using `|`. Modes also work with `ofstream` and `ifstream`.

`seekp` - method to change positon of writing inside file. Method has the following
position arguments: pure int, `ios::beg`, `ios::end`, `ios::cur` (starts from current
position).

> Using pure positioning is not easy as it counts by bytes and in UTF-8 some of the
> symbols can be more than one.

`tellp` - method to change position of reading inside file.

```cpp
    fstream fout("telefon.txt", ios::in | ios::out);

    fout.seekp(72);
    fout.seekp(-17, ios::end);
```

> Writing into file with content will overwrite each simbol from the start of
> the file by default.

`seekg` and `tellg` are methods for reading.

## binary files

`get` - use to read one char and nothing more;

`read` - use for longer files;

`getline` - to read by lines;

To work with all the content of the file is better to use buffers. Perfect buffer size
is 1Kb.

```cpp
char buffer[1024];

while (file.read(buffer, sizeof(buffer)) || file.gcount() > 0) {
    size_t n = file.gcount();
    for (size_t i = 0; i < n; ++i) {
        process(buffer[i]);
    }
}
```

Analog to `get` and `read` are `put` and `write`.

Example of `cp` implementation:

```cpp
#include <array>
#include <fstream>
#include <iostream>
#include <string>

using namespace std;

int main(int argc, const char** argv) {
    // при неверных аргументах выводим ошибку и выходим с кодом
    if (argc != 3) {
        cerr << "Usage: "s << argv[0] << " <in file> <out file>"s << endl;
        return 1;
    }

    ifstream in_file(argv[1]);
    if (!in_file) {
        cerr << "Can't open input file"s << endl;
        return 2;
    }

    ofstream out_file(argv[2]);
    if (!out_file) {
        cerr << "Can't open output file"s << endl;
        return 2;
    }

    // размер буфера один килобайт
    static const int BUFF_SIZE = 1024;
    std::array<char, BUFF_SIZE> buffer;

    do {
        in_file.read(buffer.data(), BUFF_SIZE);
        out_file.write(buffer.data(), in_file.gcount());
    } while (in_file);
}
```

Use `ios::binary` to prevent stream from fixing the read buffer. Use binary everywhere
except purely text files.
































