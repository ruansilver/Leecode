## 一阶段

### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

####解答

#####1. 

```c++

bool checkBracket(const string s) {
	stack <char> stackRuan;
	for (char temp : s) {
		if (temp == '[' || temp == '{' || temp == '(') {      //如果是左边的括号，就入栈。
			stackRuan.push(temp);
		}
		else if (temp == '}' || temp == ']' || temp == ')')
		{
			if ((temp == '}' && stackRuan.top() != '{') || (temp == ']' && stackRuan.top() != '[') || (temp == ')' && stackRuan.top() != '(')) {    //判断括号和栈顶的括号是否匹配，如果不匹配，则返回错误
				return false;
			}
			stackRuan.pop();                                 //如果匹配，则弹出栈顶元素
		}
	}
	  return stackRuan.empty();                            //现在栈是空的了
};     
```

栈，是先入先出的结构，底层基于vector、deque、list，就是数组，队列，链表，默认情况下是deque

***

> 主要特性

- **LIFO 数据结构**：`std::stack` 是一种先进后出（LIFO，Last In First Out）的数据结构，这意味着最后插入的元素将是第一个被移除的元素。
- **容器适配器**：`std::stack` 是一种容器适配器，它封装了其他序列容器，提供了一组特定的成员函数来管理堆栈中的元素。

> 主要成员函数

以下是 `std::stack` 的一些主要成员函数及其功能：

- **构造函数**：用于创建 `std::stack` 对象。
- **empty()**：检查堆栈是否为空。     空的话返回true
- **size()**：返回堆栈中元素的数量。
- **top()**：返回堆栈顶部元素的引用。
- **push(const T& val)**：将元素 `val` 压入堆栈。
- **push(T&& val)**：将元素 `val` 移动到堆栈（C++11 起）。
- **pop()**：移除堆栈顶部的元素。
- **emplace(Args&&... args)**：在堆栈顶部创建一个新元素（C++11 起）。

> 底层容器

默认情况下，`std::stack` 使用 `std::deque` 作为底层容器。可以在声明 `std::stack` 时指定其他序列容器（如 `std::vector` 或 `std::list`）作为底层容器。

```cpp
#include <iostream>
#include <stack>
#include <vector>
#include <list>

int main() {
    // 使用 std::vector 作为底层容器
    std::stack<int, std::vector<int>> stackWithVector;
    stackWithVector.push(1);
    stackWithVector.push(2);
    std::cout << "Top element with vector: " << stackWithVector.top() << std::endl;

    // 使用 std::list 作为底层容器
    std::stack<int, std::list<int>> stackWithList;
    stackWithList.push(3);
    stackWithList.push(4);
    std::cout << "Top element with list: " << stackWithList.top() << std::endl;

    return 0;
}
```

***

### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

**提示：**

- 两个链表的节点数目范围是 `[0, 50]`
- `-100 <= Node.val <= 100`
- `l1` 和 `l2` 均按 **非递减顺序** 排列

#### 题解

```c++
#include <iostream>

// Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy; // 虚拟头结点        
        ListNode* tail = &dummy; // 尾指针指向虚拟头结点
        
        while (l1 && l2) {
            if (l1->val <= l2->val) {
                tail->next = l1;
                l1 = l1->next;
            } else {
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        
        // 将剩余的节点连接到新链表中
        tail->next = l1 ? l1 : l2;              
        
        return dummy.next;        //因为dummy是一个虚拟头节点，所以返回dummy的下一个节点
    }
};

void printList(ListNode* node) {
    while (node) {
        std::cout << node->val << " ";
        node = node->next;
    }
    std::cout << std::endl;
}

int main() {
    // 创建链表 l1: [1, 2, 4]
    ListNode* l1 = new ListNode(1);
    l1->next = new ListNode(2);
    l1->next->next = new ListNode(4);
    
    // 创建链表 l2: [1, 3, 4]
    ListNode* l2 = new ListNode(1);
    l2->next = new ListNode(3);
    l2->next->next = new ListNode(4);
    
    Solution solution;
    ListNode* mergedList = solution.mergeTwoLists(l1, l2);
    
    // 输出合并后的链表
    printList(mergedList);
    
    return 0;
}

```

####双向链表List

>  什么是 `std::list`

`std::list` 是一个双向链表容器。每个节点包含一个数据域和两个指针，一个指向前一个节点，一个指向后一个节点。双向链表允许在常数时间内在序列的两端进行插入和删除操作。

> 主要特性

- **双向链表**：每个节点包含前驱和后继指针。
- **常数时间插入和删除**：在任何位置插入或删除元素的时间复杂度为 O(1)。
- **线性时间访问**：访问元素的时间复杂度为 O(n)。
- **不支持随机访问**：不像 `std::vector`，`std::list` 不支持 `operator[]` 或 `at()` 进行随机访问。

​    要使用 `std::list`，需要包含头文件 `<list>`

> 创建和初始化

可以通过多种方式创建和初始化 `std::list`：

```cpp
#include <iostream>
#include <list>

int main() {
    // 创建一个空的 std::list
    std::list<int> mylist;

    // 用初始值列表初始化
    std::list<int> mylist2 = {1, 2, 3, 4, 5};

    // 使用特定数量的默认值初始化
    std::list<int> mylist3(5, 10); // 5 个元素，每个值为 10

    // 复制构造函数
    std::list<int> mylist4(mylist2);

    return 0;
}
```

> 常用成员函数

**插入和删除**

- `push_back(const T& val)`: 在列表末尾插入元素。
- `push_front(const T& val)`: 在列表开头插入元素。
- `pop_back()`: 删除列表末尾的元素。
- `pop_front()`: 删除列表开头的元素。
- `insert(iterator pos, const T& val)`: 在指定位置插入元素。
- `erase(iterator pos)`: 删除指定位置的元素。
- `clear()`: 清空列表。

**访问元素**

- `front()`: 返回列表第一个元素的引用。
- `back()`: 返回列表最后一个元素的引用。

**其他操作**

- `size()`: 返回列表中元素的数量。
- `empty()`: 检查列表是否为空。
- `sort()`: 对列表元素进行排序。
- `reverse()`: 反转列表元素的顺序。

> 示例代码

以下是一些示例代码，展示了如何使用 `std::list` 进行基本操作：

```cpp
#include <iostream>
#include <list>

int main() {
    // 创建一个 std::list 并插入一些元素
    std::list<int> mylist = {2, 1, 4, 3};

    // 在列表末尾插入元素
    mylist.push_back(5);

    // 在列表开头插入元素
    mylist.push_front(0);

    // 遍历并打印列表元素
    for (int val : mylist) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    // 删除列表开头的元素
    mylist.pop_front();

    // 删除列表末尾的元素
    mylist.pop_back();

    // 使用迭代器插入元素
    auto it = mylist.begin();
    std::advance(it, 2);
    mylist.insert(it, 8);

    // 使用迭代器删除元素
    mylist.erase(it);

    // 排序列表
    mylist.sort();

    // 反转列表
    mylist.reverse();

    // 打印修改后的列表
    for (int val : mylist) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

而在次题目，用的是单向链表

#### 单向链表

 单链表（Singly Linked List）是一种链式数据结构，其中每个节点包含数据和指向下一个节点的指针。与数组不同，单链表的节点不必在内存中连续存储。

> 单链表的结构

在单链表中，每个节点包含两个部分：
1. **数据部分**：存储节点的数据。
2. **指针部分**：存储指向下一个节点的指针。

链表的最后一个节点指向 `nullptr`，表示链表的结束。

> 单链表的基本操作

#### 节点的定义

在 C++ 中，可以通过定义一个结构体或类来表示单链表的节点：

```cpp
struct Node {
    int data;      // 数据部分
    Node* next;    // 指针部分
    Node(int val) : data(val), next(nullptr) {}
};
```

#### 创建和初始化单链表

以下是如何创建和初始化单链表的示例：

```cpp
#include <iostream>

struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}          //构造函数，头节点
};

int main() {
    // 创建单向链表的节点
    Node* head = new Node(1); // 头节点
    head->next = new Node(2); // 第二个节点
    head->next->next = new Node(3); // 第三个节点

    // 打印链表中的元素
    Node* current = head;
    while (current != nullptr) {
        std::cout << current->data << " ";
        current = current->next;
    }

    // 释放链表中的节点
    while (head != nullptr) {
        Node* temp = head;
        head = head->next;
        delete temp;
    }

    return 0;
}
```

> 插入节点

在单链表的开头、中间和结尾插入节点的示例：

```cpp
void insertAtBeginning(Node*& head, int newData) {      //从头部插入
    Node* newNode = new Node(newData);
    newNode->next = head;
    head = newNode;
}

void insertAfter(Node* prevNode, int newData) {         //从某个位置插入
    if (prevNode == nullptr) {                          //首先判断插入的这个位置是不是空指针
        std::cout << "Previous node cannot be null." << std::endl;
        return;
    }
    Node* newNode = new Node(newData);
    newNode->next = prevNode->next;
    prevNode->next = newNode;
}
//从尾部插入
void insertAtEnd(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    if (head == nullptr) {            //如果是一个空的链表,要单独写出来
        head = newNode;               //因为空的链表的指针是指向的nullptr，也就是空指针，空指针的下一个指向
        return;
    }
    Node* last = head;            //用last而不是直接移动head，是为了让last去找链表最后面的那个节点
    while (last->next != nullptr) {
        last = last->next;
    }
    last->next = newNode;
}
```

> 删除节点

删除单链表中的节点示例：

```cpp
void deleteNode(Node*& head, int key) {   //这个key是按值搜索用的关键字
    Node* temp = head;                    //创建一个搜索用的临时指针，初始化搜索指针指向链表的头节点
    Node* prev = nullptr;                 //这个prev用来指代temp的前面的那个节点

    // 如果头节点本身是要删除的节点
    if (temp != nullptr && temp->data == key) {
        head = temp->next;                     //头指针往后移动
        delete temp;                           //删除这个节点
        return;
    }

    // 搜索要删除的节点
    while (temp != nullptr && temp->data != key) {  //条件：删除的临时指针不断在链表中后移，当且仅当没有到达链表末尾（空指向）以及与要删除的值不匹配的时候。   都进行搜索
        prev = temp;                    //这个prev也跟随着不断移动，永远指向temp的前面的那个节点
        temp = temp->next;
    }

    // 如果找不到要删除的节点
    if (temp == nullptr) {
        return;
    }

    // 从链表中删除节点
    prev->next = temp->next;             //直接跳过这个要删除的节点
    delete temp;                         //删除这个搜索用的临时指针
}
```

> 单链表的优势和劣势

**优势**

1. **动态大小**：链表大小可以动态增长或缩减，不需要预先定义大小。
2. **高效的插入和删除操作**：在已知位置进行插入和删除操作只需要常数时间。

**劣势**

1. **顺序访问**：链表不支持随机访问，查找某个特定元素需要线性时间。
2. **额外内存开销**：每个节点需要额外的指针内存。
3. **复杂性**：实现链表操作相对复杂，需要处理指针操作和边界情况。

> 完整示例代码

以下是一个完整的示例，展示了如何创建、插入、删除和遍历单链表：

```cpp
#include <iostream>

struct Node {
    int data;
    Node* next;

    Node(int val) : data(val), next(nullptr) {}
};

void insertAtBeginning(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    newNode->next = head;
    head = newNode;
}

void insertAfter(Node* prevNode, int newData) {
    if (prevNode == nullptr) {
        std::cout << "Previous node cannot be null." << std::endl;
        return;
    }
    Node* newNode = new Node(newData);
    newNode->next = prevNode->next;
    prevNode->next = newNode;
}

void insertAtEnd(Node*& head, int newData) {
    Node* newNode = new Node(newData);
    if (head == nullptr) {
        head = newNode;
        return;
    }
    Node* last = head;
    while (last->next != nullptr) {
        last = last->next;
    }
    last->next = newNode;
}

void deleteNode(Node*& head, int key) {
    Node* temp = head;
    Node* prev = nullptr;

    // 如果头节点本身是要删除的节点
    if (temp != nullptr && temp->data == key) {
        head = temp->next;
        delete temp;
        return;
    }

    // 搜索要删除的节点
    while (temp != nullptr && temp->data != key) {
        prev = temp;
        temp = temp->next;
    }

    // 如果找不到要删除的节点
    if (temp == nullptr) {
        return;
    }

    // 从链表中删除节点
    prev->next = temp->next;
    delete temp;
}

void printList(Node* node) {
    while (node != nullptr) {
        std::cout << node->data << " ";
        node = node->next;
    }
    std::cout << std::endl;
}

int main() {
    Node* head = nullptr;    //创建一个头指针节点，它的指向为空

    insertAtEnd(head, 1);
    insertAtEnd(head, 2);
    insertAtEnd(head, 3);
    insertAtBeginning(head, 0);
    insertAfter(head->next, 5);

    std::cout << "Linked list: ";
    printList(head);

    deleteNode(head, 2);
    std::cout << "After deletion of 2: ";
    printList(head);

    // 释放链表中的节点
    while (head != nullptr) {
        Node* temp = head;
        head = head->next;
        delete temp;
    }

    return 0;
}
```

在这个示例中，我们展示了如何定义单链表节点，如何在链表的开头、中间和结尾插入节点，如何删除节点，以及如何遍历并打印链表中的元素。

***

### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

#### 题解：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0; // 如果数组为空，直接返回 0

        int i = 0; // 用于指向当前处理好的唯一元素的末尾
        for (int j = 1; j < nums.size(); j++) { // 快指针，遍历整个数组
            if (nums[j] != nums[i]) { // 如果当前元素和慢指针指向的元素不同
                i++; // 慢指针向前移动一位
                nums[i] = nums[j]; // 将当前元素赋值给慢指针新位置
            }
        }

        return i + 1; // 返回唯一元素的个数
    }
};
```

***

####Vector容器的介绍

`std::vector` 是 C++ 标准库中的一个动态数组容器，能够在运行时自动调整大小。它提供了许多便利的方法来操作元素，是 C++ 中常用的数据结构之一。

> 特点和优势：

1. **动态大小**：
   - `std::vector` 可以动态增长或缩减，根据需要自动调整存储空间大小。
   - 当向 `vector` 添加元素时，如果当前存储空间不足，`vector` 会自动分配更多内存，以容纳新元素。

2. **随机访问**：
   - `std::vector` 支持通过索引直接访问元素，具有常数时间复杂度（O(1)）。
   - 可以使用 `operator[]` 或 `at()` 方法来访问元素。

3. **元素存储连续**：
   - `std::vector` 的元素在内存中是连续存储的，这有利于缓存性能。

4. **标准库支持**：
   - `std::vector` 是 C++ 标准库中的一部分，提供了丰富的操作和算法，如排序、查找等。

> 使用示例：

**创建和初始化 `vector`**

```cpp
#include <iostream>
#include <vector>

int main() {
    // 创建一个空的 vector
    std::vector<int> vec1;

    // 初始化带有初始值的 vector
    std::vector<int> vec2 = {1, 2, 3, 4, 5};

    // 添加元素到 vector
    vec1.push_back(10);
    vec1.push_back(20);
    vec1.push_back(30);

    // 访问 vector 中的元素
    std::cout << "vec1 elements:";
    for (int i = 0; i < vec1.size(); ++i) {
        std::cout << " " << vec1[i];
    }
    std::cout << std::endl;

    std::cout << "vec2 elements:";
    for (auto& elem : vec2) {
        std::cout << " " << elem;
    }
    std::cout << std::endl;

    return 0;
}
```

> 常用操作：

- **访问元素**：使用 `operator[]` 或 `at()` 方法。
  
  ```cpp
  int value = vec1[0]; // 访问第一个元素
  ```

- **添加元素**：使用 `push_back()` 方法在尾部添加元素。

  ```cpp
  vec1.push_back(40); // 在尾部添加元素 40
  ```

- **删除元素**：使用 `pop_back()` 方法删除尾部元素。

  ```cpp
  vec1.pop_back(); // 删除尾部元素
  ```

- **获取大小**：使用 `size()` 方法获取 `vector` 的当前大小。

  ```cpp
  int size = vec1.size(); // 获取 vector 的大小
  ```

- **遍历元素**：使用范围-based for 循环或迭代器遍历 `vector` 中的元素。

  ```cpp
  for (auto& elem : vec2) {
      std::cout << elem << " ";
  }
  ```

***

###[27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

#### 题解：

```c++
class Solution {
public:
	int removeElement(vector<int>& nums, int val) {
		int i = 0;     //慢指针
		for (int j = 0; j < nums.size(); j++) {
			nums[i] = nums[j];
			if (nums[i] != val) {
				i++;
			}
		}
		return i;
	}
};
```

***

### [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

**提示：**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

#### 题解：

```c++
```

#### KMP算法讲解

KMP算法（Knuth-Morris-Pratt算法）是一种用于在字符串中查找子串的高效算法。它在进行匹配时避免了重复的比较，因此在最坏情况下时间复杂度为O(n + m)，其中n是主字符串的长度，m是子串的长度。

KMP算法的核心思想是通过预处理模式串，构建一个部分匹配表（又称为失配函数、前缀表、Next数组），用来记录模式串的前缀和后缀的匹配情况。这样，当出现不匹配时，可以根据前缀表跳过一些不必要的匹配，从而提高效率。

> KMP算法步骤

1. **构建前缀表（Next数组）**：
   - 计算模式串的前缀和后缀的最长公共部分长度，用数组`next`记录。
   - `next[i]`表示模式串中前`i`个字符的最长相等前缀后缀的长度。

2. **利用前缀表进行匹配**：
   - 使用`next`数组在主字符串中查找模式串，遇到不匹配时，利用`next`数组跳过一些字符继续匹配。

> 构建前缀表（Next数组）

假设模式串为`pattern`，长度为`m`。

1. 初始化`next[0] = -1`，`j = -1`。
2. 从`i = 1`开始，逐个计算`next[i]`：
   - 如果`j == -1`或`pattern[i - 1] == pattern[j]`，则`j++`，然后`next[i] = j`。
   - 否则，更新`j = next[j]`，直到`pattern[i - 1] == pattern[j]`或`j == -1`。

> 匹配过程

假设主字符串为`text`，长度为`n`，模式串为`pattern`，长度为`m`。

1. 初始化`i = 0`（主字符串的指针），`j = 0`（模式串的指针）。
2. 当`i < n`时，逐个字符进行比较：
   - 如果`j == -1`或`text[i] == pattern[j]`，则`i++`，`j++`。
   - 否则，更新`j = next[j]`，继续比较。
3. 如果`j == m`，说明找到了一个匹配，记录匹配位置`i - j`，然后更新`j = next[j]`，继续查找。

> 代码实现

```cpp
#include <vector>
#include <string>
#include <iostream>

using namespace std;

// 构建前缀表
vector<int> buildPrefixTable(const string &pattern) {
    int m = pattern.size();
    vector<int> next(m, -1);
    int j = -1;
    for (int i = 1; i < m; i++) {
        while (j >= 0 && pattern[i - 1] != pattern[j]) {
            j = next[j];
        }
        j++;
        next[i] = j;
    }
    return next;
}

// KMP匹配过程
vector<int> KMP(const string &text, const string &pattern) {
    vector<int> result;
    if (pattern.empty()) return result;
    vector<int> next = buildPrefixTable(pattern);
    int n = text.size();
    int m = pattern.size();
    int i = 0, j = 0;
    while (i < n) {
        if (j == -1 || text[i] == pattern[j]) {
            i++;
            j++;
        } else {
            j = next[j];
        }
        if (j == m) {
            result.push_back(i - j);
            j = next[j];
        }
    }
    return result;
}

int main() {
    string text = "ababcabcababcabc";
    string pattern = "abc";
    vector<int> positions = KMP(text, pattern);
    for (int pos : positions) {
        cout << "Pattern found at position " << pos << endl;
    }
    return 0;
}
```

> 解释

1. **buildPrefixTable**：构建前缀表，计算每个位置的最长前缀后缀长度。
2. **KMP**：利用前缀表进行模式匹配，找到所有匹配的位置。
3. **主程序**：示例使用KMP算法在主字符串中查找模式串，并输出所有匹配位置。

通过这种方式，KMP算法能够高效地在主字符串中找到所有匹配的子串位置。
