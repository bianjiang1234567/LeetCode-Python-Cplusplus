# 题目

Implement a MyCalendarThree class to store your events. A new event can always be added.

Your class will have one method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A K-booking happens when K events have some non-empty intersection (ie., there is some time that is common to all K events.)

For each call to the method MyCalendar.book, return an integer K representing the largest integer such that there exists a K-booking in the calendar.

Your class will be called like this: MyCalendarThree cal = new MyCalendarThree(); MyCalendarThree.book(start, end)


#### Example 1:
```
MyCalendarThree();
MyCalendarThree.book(10, 20); // returns 1
MyCalendarThree.book(50, 60); // returns 1
MyCalendarThree.book(10, 40); // returns 2
MyCalendarThree.book(5, 15); // returns 3
MyCalendarThree.book(5, 10); // returns 3
MyCalendarThree.book(25, 55); // returns 3
Explanation:
The first two events can be booked and are disjoint, so the maximum K-booking is a 1-booking.
The third event [10, 40) intersects the first event, and the maximum K-booking is a 2-booking.
The remaining events cause the maximum K-booking to be only a 3-booking.
Note that the last event locally causes a 2-booking, but the answer is still 3 because
eg. [10, 20), [10, 40), and [5, 15) are still triple booked.
```

#### Note:

* The number of calls to MyCalendarThree.book per test case will be at most 400.
* In calls to MyCalendarThree.book(start, end), start and end are integers in the range [0, 10^9].

# Python3 Solution
## 解题思路1：
用一个排序字典存储区间端点和对应出现的次数，start +1，end -1，遍历之后记录最大的重复区间次数
```
class MyCalendarThree:

    def __init__(self):
        self.dic = collections.OrderedDict()

    def book(self, start: int, end: int) -> int:
        self.dic[start] = self.dic.get(start, 0) + 1
        self.dic[end] = self.dic.get(end, 0) - 1
        mx, num = 0, 0
        dic = sorted(self.dic.items(), key = lambda x : x[0])
        for i, j in dic:#返回dic是一个列表
            num += j
            mx = max(mx, num)
        return mx

# Your MyCalendarThree object will be instantiated and called as such:
# obj = MyCalendarThree()
# param_1 = obj.book(start,end)
```

# C++ Solution
## 解题思路：
C++中的map具有自动排序功能，因为实现的数据结构为树

```
class MyCalendarThree {
public:
    MyCalendarThree() {

    }

    int book(int start, int end) {
        ++frequency[start];
        --frequency[end];
        int mx=0, num = 0;
        for(auto dot : frequency)
        {
            num += dot.second;
            mx = max(mx, num);
        }
        return mx;
    }

private:
    map<int, int> frequency;
};

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree* obj = new MyCalendarThree();
 * int param_1 = obj->book(start,end);
 */
```
