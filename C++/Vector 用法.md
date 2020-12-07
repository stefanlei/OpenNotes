#### Vector 用法

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {

    vector<int> v;

    v.reserve(10);
    for (int i = 0; i < 10; i++) {
        // 尾部添加元素
        v.push_back(i);
    }
    cout << "size: " << v.size() << endl;

    // 迭代写法
    for (int i:v) {
//        cout << i << endl;
    }

    // 普通循环
    for (int i = 0; i < v.size(); i++) {
//        cout << v[i] << endl;
    }

    // 插入元素
    v.insert(v.begin(), 333);


    // 删除元素 删除第3个元素
    v.erase(v.begin() + 2);

    // 删除区间 [begin,end-1] 区间里面的元素 ,相当于全部删除了
    v.erase(v.begin(), v.end());


    v.push_back(1);
    v.push_back(2);
    v.push_back(3);


    for (int i:v) {
        cout << i << endl;
    }

    // 清空元素
    v.clear();


}

```

