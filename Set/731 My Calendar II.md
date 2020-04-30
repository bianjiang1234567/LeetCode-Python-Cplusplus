# 题目

Implement a MyCalendarTwo class to store your events. A new event can be added if adding the event will not cause a triple booking.

Your class will have one method, book(int start, int end). Formally, this represents a booking on the half open interval [start, end), the range of real numbers x such that start <= x < end.

A triple booking happens when three events have some non-empty intersection (ie., there is some time that is common to all 3 events.)

For each call to the method MyCalendar.book, return true if the event can be added to the calendar successfully without causing a triple booking. Otherwise, return false and do not add the event to the calendar.

Your class will be called like this: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)


#### Example 1:
```
MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(50, 60); // returns true
MyCalendar.book(10, 40); // returns true
MyCalendar.book(5, 15); // returns false
MyCalendar.book(5, 10); // returns true
MyCalendar.book(25, 55); // returns true
Explanation:
The first two events can be booked.  The third event can be double booked.
The fourth event (5, 15) can't be booked, because it would result in a triple booking.
The fifth event (5, 10) can be booked, as it does not use time 10 which is already double booked.
The sixth event (25, 55) can be booked, as the time in [25, 40) will be double booked with the third event;
the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
```

#### Note:

* The number of calls to MyCalendar.book per test case will be at most 1000.
* In calls to MyCalendar.book(start, end), start and end are integers in the range [0, 10^9].

# Python3 Solution
## 解题思路：
用两个set记录重叠两次的区间和只有一次的区间，先在重叠两次的区间里面搜索，若有重叠则返回false，然后在只有一次的区间搜索，并更新两个set
```
class MyCalendarTwo:

    def __init__(self):
        self.set1 = set()
        self.set2 = set()

    def book(self, start: int, end: int) -> bool:
        for a in self.set2:
            if start>=a[1] or end <= a[0]:
                continue
            return False
        for a in self.set1:
            if start>=a[1] or end<=a[0]:
                continue
            self.set2.add((max(start,a[0]), min(end,a[1])))
        self.set1.add((start, end))
        return True


# Your MyCalendarTwo object will be instantiated and called as such:
# obj = MyCalendarTwo()
# param_1 = obj.book(start,end)
```

# C++ Solution
## 解题思路：
同上

```
class MyCalendarTwo {
public:
    MyCalendarTwo() {

    }

    bool book(int start, int end) {
        for(auto a : s2)
        {
            if(start>=a.second || end<=a.first ) continue;
            else
                return false;
        }
        for(auto a : s1)
        {
            if(start>=a.second || end<=a.first) continue;
            s2.insert(pair<int, int>{max(start, a.first), min(end, a.second)});
        }
        s1.insert(pair<int, int>{start, end});
        return true;
    }
private:
    set<pair<int, int>> s1, s2;//s1存出现一次区间，s2存重叠区间
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo* obj = new MyCalendarTwo();
 * bool param_1 = obj->book(start,end);
 */
```
