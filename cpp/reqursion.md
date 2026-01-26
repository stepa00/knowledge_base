# Reqursion

```cpp
int Factorial(int n) {
    // Для n, равного единице, функция сразу возвращает результат.
    if (n == 1) {
        return 1;
    }
    // В остальных случаях факториал n можно 
    // выразить через факториал числа n - 1.
    const int prev_factorial = Factorial(n - 1);
    return n * prev_factorial ;
}
```

> Many reqursions will be slow.

```cpp
std::vector<int> MergeSort(const std::vector<int>& arr) {
    // Корнер-кейс. Массивы размером 0 или 1 уже отсортированы.
    if (arr.size() <= 1) {
        return arr;
    }
    // Исходный массив делится пополам.
    // Для каждой половины повторно вызывается алгоритм сортировки.
    const size_t mid = arr.size() / 2;
    const std::vector<int> left_sorted = MergeSort({arr.begin(), arr.begin() + mid});
    const std::vector<int> right_sorted = MergeSort({arr.begin() + mid, arr.end()});
    
    // left_sorted и right_sorted – отсортированные векторы.
    // Сливаем их и получаем из двух отсортированных векторов один.
  return Merge(left_sorted, right_sorted);
}
```

```cpp
// Другой корнер-кейс.
if (arr.size() <= 8) {
        return InsertionSort(arr);
}
```


