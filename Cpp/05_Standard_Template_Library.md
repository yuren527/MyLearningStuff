# Vectors
`Vector`可以像`array`一样指定长度
```C++
vector<string> strings(5);
```
# Vector and memory
A vector actually contains a internal array, we can initialize the size of the internal array with:
> vector\<int\> test(20);  
    
This initialize a internal array with the size of `20`  

But, ***once we add new element to the vector that is beyond the original size, a new internal array with twice the original size is created, and it will copy the old elements.***   

So, we should know that the difference between `size()` and `capacity()`.One is the length of the vector and the other indicates the size of the internal array.   

**Some of the functions should be highlightened:**  
- `clear()` is to dicard all the elements in the vector, but do not change the size or capacity of the vector.  
- `resize()` means to give a new size of the vector.  
- `reserve()` means to give a new capacity of the internal array, which can only be increased.  
# Two-Dimensional Vectors
**Example**
```C++
#include <iostream>
#include <vector>
using namespace std;

int main()
{
    vector< vector<int> > grid(3, vector<int>(4, 7));
    grid[1].push_back(8);
    
    for(int row = 0; row < grid.size(); row++)
    {
        for(int col = 0; col < grod[row].size(); col++)
        {
            cout << grid[row][col] << flush;
        }
        cout << endl;
    }
    return 0;
}
```
then we get the test result
> 7777  
> 77778  
> 7777

**Notice `vector< vector<int> >`, we used two spaces between the outer <> and the inner vector, that's because without it, the compiler will posibly misunderstand the code, so this is for easier reading by compiler.**
# Lists
Quite similar to `vector`  
Also called `double-linked list`  
**But there is some differences between them:**  
- Element can be inserted in the middle of a `list` while it's restricted to insert element at the begin or the end in a `vector`  
- We cannot use a index to access elements in a list, use `iterator` instead    
- The data storage in `vector` is continuous while it is not in `list`  
- Elements in `list` is actual nodes which has a pointer to the memory block of the previous data and a pointer to the next  

**Delete Element:**  
To delete element in a `list`, use:  
`it = list.erase(it);`  
Because it's unreliable to access iterator after calling `erase()`, the validation of iterator depends on the chance of whether the memory is reused. Notice that, this actually increased the iterator;  
# Maps
Let's see the example:  
```C++
#include <iostream>
#include <map>
using namespace std;

int main()
{
    map<string, int> ages;

    ages["Mike"] = 40;
    ages["Raj"] = 20;
    age["Vicky"] = 30;

    ages["Mike"] = 70;

    ages.insert(make_pair("Peter", 100));

    cout << ages["Raj"] << endl;

    if(ages.find("Vicky") != ages.end())
    {
        cout << "Found Vicky" << endl;
    }
    else
    {
        cout << "Key Not Found" << endl;
    }

    for(map<string, int>::iterator it = ages.begin(); it != ages.end(); age++)
    {
        pair<string, int> age = *it;
        cout << age.first << ": " << age.second << endl;
    }
    return 0;
}
```
- `make_pair()` can be used to make pairs instead of `pair<string, int>()`  
- Keys in map must be unique, so when we assign a new key which already exists in the map, it will replace the old one rather than adding a new one.
- If `find()` a non-existing key, it returns the end of the map: `ages.end()`

**If use a custom object as a map key, compare operator should be overloaded because the map needs to order the keys**
# Multimaps
Let's see the example
```C++
#include <iostream>
#include <map>
using namespace std;

int main()
{
    multimap<int, string> lookup;

    lookup.insert(make_pair(30, "Mike"));
    lookup.insert(make_pair(10, "Vicky"));
    lookup.insert(make_pair(30, "Raj"));
    lookup.insert(make_pair(20, "Bob"));

    for(multimap<int, string>::iterator it = lookup.begin(); it != lookup.end(); it++)
    {
        cout << it->first << ": " << it->second << endl;
    }
    cout << endl;

    pair<multimap<int, string>::iterator, multimap<int, string>::iteraor> its = lookup.equal_range(30);
    for(multimap<int, string>::iterator it = its.first; it != its.second; it++)
    {
        cout << it->first << ": " << it->second << endl;
    }
    cout << endl;

    auto its2 = lookup.equal_range(30);
    for(multimap<int, string>::iterator it = its2.first; it != its2.second; it++)
    {
        cout << it->first << ": " << it->second << endl;
    }

    return 0;
}
```
- A multimap can use non-unique keys
- Remember the use of method `equal_range()`
- In C++11, a new feature has been added, we can use `auto` instead of many type declare, to make the code brief and elegant
  
# Set
A set can only stores unique elements and **orders them**   

# Stacks and queues
Stack and queue are very similar.
```C++
class Test
{
    string name;
    Test(string s) : name(s){}
}
int main()
{
    stack<Test> testStack;
    testStack.push(Test("Mike"));
	
    return 0;
}
```
If we do like above, a `Test` that we created when pushing into the stack will be destroyed.

If want to iterate through the stack or queue, use a for loop and `pop()` one by one. Use references to reference to the elements if we dont need them to persist.

# Sort and deque
**A deque is very similar to `vector`, but the `deque` can push/pop element to/from the front as well as to the back**

Remember the use of function `sort(test.begin(), test.end(), comp)`  
`comp` is a function pointer.
# Complex date type
Nested collection data type is really confusing, recommend to use a class which encapsule collections.

See the example below:
```C++
#include <iostream>
#include <map>
#include <vector>
using namespace std;

int main()
{
    map<string, vector<int> > scores;

    scores["Mike"].push_back(10);
    scores["Mike"].push_back(30);
    scores["Vicky"].push_back(20);

    for(map<string, vector<int> >::iterator it = scores.begin(); it != scores.end(); it++)
    {
        string name = it->first;
        vector<int> scoreList = it->second;
        cout << name << ": " << flush;
        for(int i = 0; i < scoreList.size(); i++)
        {
            cout << scoreList[i] << " " << flush;
        }
        cout << endl;
    }
    return 0;
}
```