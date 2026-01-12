# Stack

`std::stack` - template container, here only last element can be
changed.

- push - add element;

- pop - delete last element;

- top - read or change last element.

Last element - **top of the stack**.

```cpp
// Создание пустого стека с элементами типа int.
std::stack<int> my_stack;
// Можно добавлять элементы на вершину стека.
my_stack.push(100);
my_stack.push(200);

// Или посмотреть, какой элемент сейчас сверху. Это не удаляет его из стека.
std::cout << my_stack.top() << std::endl; // 200.

// И удалить последний элемент.
my_stack.pop();

// Теперь остался только один элемент — 100.
std::cout << my_stack.size() << " "s << my_stack.top() << std::endl; // 1 100

// Так можно поменять вершину.
my_stack.top() = 42;
```

> LIFO - last in - first out.

```cpp
// В этом стеке будут храниться цвета неудалённых птиц.
std::stack<int> birds;

int color;
while (std::cin >> color) {
    // Для текущей птицы цвета color.
    // Если её цвет совпадает с предыдущим цветом, 
    // нужно удалить предыдущий цвет и не добавлять текущий.
    if (!birds.empty() && birds.top() == color) {
        birds.pop();
    } else {
        // Если предыдущий не совпадает (или его нет), нужно добавить текущий.
        birds.push(color);
    }
}
// В стеке хранятся неудалённые птицы (цвета). Их количество и есть ответ.
std::cout << birds.size() << std::endl;
```

# Queue

> FIFO - first in - first out

- push - add to the start of the queue;

- pop - remove from the end of the queue.

```cpp
// Создание пустой очереди с элементами типа int.
std::queue<int> queue;
// Можно добавлять элементы в конец очереди.
queue.push(100);
queue.push(200);
// Или посмотреть в начале. Это не удаляет элемент из очереди.
std::cout << queue.front() << std::endl; // 100.
// И удалить первый элемент.
queue.pop();
// Теперь остался только один элемент.
std::cout << queue.size() << " "s << queue.front() << std::endl; // 1 200
```

# Task scheduler

This example is how operation systems function. Each task get some time
and than put back into queue if not finished. Next task taken and so on.

```cpp
template <class Task>
class Scheduler {
public:
    void PushTask(const Task& task) {
        // При добавлении новой задачи добавляем её в конец очереди.
        queue_.push(task);
    }
    
    bool DoWork() {
        if (queue_.empty()) {
            // Если задач нет, сообщаем, что вся работа выполнена.
            return false;
        }
        
        // Берём первую задачу в очереди и делаем её часть.
        auto task = queue_.front();
        queue.pop();
        task.DoSomeWork();
        
        // Если задача не выполнена, добавляем её в конец очереди.
        if (!task.Finished()) {
            queue_.push(task);
        }
        
        return true;
    }
  
private:
    std::queue<Task> queue_;
};
```

# Deque

`std::deque`

```cpp
std::deque<int> deque;
// Такой же интерфейс, как у вектора для работы с последними элементами.
deque.push_back(2);
// Но ещё есть эффективные методы для добавления и удаления первых элементов.
deque.push_front(1);
std::cout << deque.front() << " "s << deque.back() << std::endl; // 1 2

// Удалим последний элемент. Останется всего один: начало и конец одинаковые.
deque.pop_back();
std::cout << deque.front() << " "s << deque.back() << std::endl; // 1 1
```

`std::list` is double linked list , where elements linked both ways with pointers.
But `std::deque` is faster.


