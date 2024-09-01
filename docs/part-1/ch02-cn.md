# 第2章 掌握std::vector迭代器


## 2.1 STL中迭代器的类型

### 2.1.1 输入迭代器（Input iterators）
输入迭代器是迭代器类型的基础和核心。其主要功能是：
- 读取元素：允许开发者读取容器中元素的值。
- 遍历元素：支持迭代器向前移动，以访问容器中的后续元素。
- 单向遍历：一旦输入迭代器向前移动到下一个元素，就无法返回到前一个元素。
- 不可修改性：输入迭代器不支持对当前元素的修改。

- 原文example
```cpp
#include <iostream>
#include <vector>

int main() {
  std::vector<int> numbers = {10, 20, 30, 40, 50};

  for (auto it = numbers.begin(); it != numbers.end();
       ++it) {
    std::cout << *it << " ";
  }
  std::cout << "\n";

  return 0;
}

```

### 2.1.2 输出迭代器（Output iterators）
输出迭代器只能用于写入数据，不支持通过输出迭代器直接读取所引用的元素。

- 原文example
```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main() {
  std::vector<int> numbers;

  std::generate_n(std::back_inserter(numbers), 10,
                  [n = 0]() mutable { return ++n; });

  for (auto num : numbers) { std::cout << num << " "; }
  std::cout << "\n";

  return 0;
}
```
上面代码中，`std::back_inserter`得到的是一个输出迭代器适配器，它专为`std::vector`等容器设计。使用`std::back_inserter`，可以将新值写入`vector`的末尾。

### 2.1.3 前向迭代器（Forward iterators）
向前迭代器结合了输入迭代器和输出迭代器的功能。支持读写操作，并且正如其名称所暗示的，总是向前移动。
向前迭代器从不会改变其方向。非常适合操作单链表（如std::forward_list）中的算法。

- example
```cpp
#include <forward_list>
#include <iostream>

int main() 
{
  std::forward_list<int> flist = {10, 20, 30, 40, 50};

  std::cout << "Original list: ";
  for (auto it = flist.begin(); it != flist.end(); ++it) {
    std::cout << *it << " ";
  }
  std::cout << "\n";

  for (auto it = flist.begin(); it != flist.end(); ++it) {
    (*it)++;
  }

  std::cout << "Modified list: ";
  for (auto it = flist.begin(); it != flist.end(); ++it) {
    std::cout << *it << " ";
  }
  std::cout << "\n";

  return 0;
}
```
-  程序输出
```
Original list: 10 20 30 40 50
Modified list: 11 21 31 41 51
```

### 2.1.4 反向迭代器（Reverse iterators）
反向迭代器（reverse iterators）在C++中用于以反向顺序遍历容器，如向量（vector）。这种遍历方式在特定算法和数据处理任务中非常有用。

主要的反向迭代器函数有：
- rbegin()：返回一个反向迭代器，指向容器的最后一个元素。
- rend()：返回一个反向迭代器，指向容器的第一个元素之前的位置。

这些迭代器实际上是一种迭代器适配器（iterator adaptor）。例如，`std::reverse_iterator` 是一个迭代器适配器，它接受一个双向迭代器（双向迭代器或满足C++20引入的双向迭代器标准的迭代器），并反转其遍历方向。

### 2.1.5 双向迭代器
双向迭代器支持读写操作，可以双向遍历。典型的双向迭代器有 `std::list` 和 `std::set`。

- example
```cpp
#include <iostream>
#include <list>

int main() {
  std::list<int> numbers = {1, 2, 3, 4, 5};

  std::cout << "Traversing the list forwards:\n";
  for (std::list<int>::iterator it = numbers.begin();
       it != numbers.end(); ++it) {
    std::cout << *it << " ";
  }
  std::cout << "\n";

  std::cout << "Traversing the list backwards:\n";
  for (std::list<int>::reverse_iterator rit =
           numbers.rbegin();
       rit != numbers.rend(); ++rit) {
    std::cout << *rit << " ";
  }
  std::cout << "\n";

  return 0;
}
```
- 程序输出
```
Traversing the list forwards:
1 2 3 4 5
Traversing the list backward:
5 4 3 2 1
```

### 2.1.5 随机访问迭代器（Random access iterators）
随机访问迭代器支持读写操作，可以双向遍历，并且支持随机访问。

- example 
```cpp
#include <chrono>
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

std::mutex vecMutex;

void add_to_vector(std::vector<int> &numbers, int value) {
  std::lock_guard<std::mutex> guard(vecMutex);
  numbers.push_back(value);
}

void print_vector(const std::vector<int> &numbers) {
  std::lock_guard<std::mutex> guard(vecMutex);
  for (int num : numbers) { std::cout << num << " "; }
  std::cout << "\n";
}

int main() {
  std::vector<int> numbers;
  std::thread t1(add_to_vector, std::ref(numbers), 1);
  std::thread t2(add_to_vector, std::ref(numbers), 2);
  t1.join();
  t2.join();
  std::thread t3(print_vector, std::ref(numbers));
  t3.join();
  return 0;
}
```

## 2.2

## 2.3

## 2.4

## 2.5

## 2.6

## 2.7 小结

