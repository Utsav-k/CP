### Majority Element
Problem - https://leetcode.com/problems/majority-element/description/

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

```cpp
class Solution {
public:
    int majorityElement(vector<int>& a) {
        int ans = a[0];
        int count = 0;
        for(int i=0; i<a.size(); ++i) {
            if(count == 0) ans = a[i];
            if(ans == a[i]) count++;
            else count--;
        }
        return ans;
    }
};

```
________
### Kadane's Algorithm
// TODO - Read about this!

### Sort Colours (Dutch national flag algorithm)
_________
```cpp
class Solution {
public:
    void sortColors(vector<int>& a) {
        int l = 0, r = a.size()-1;
        int i = 0;
        while(i<=r) {
            if(a[i] == 0) {
                swap(a[i], a[l]);
                ++i; ++l;
            }
            else if(a[i] == 1) {
                i++;
            }
            else if(a[i] == 2) {
                swap(a[i], a[r]);
                --r;
            }
        }
    }
};
```
____________
### Trap Rain Water 
https://leetcode.com/problems/trapping-rain-water/description
```cpp

/*
This idea is to maintain the maxToLeft and maxToRight for any index. Can be done using 2 arrays to precompute 2 arrays and use them. This is the on fly variant. 
*/
class Solution {
public:
    int trap(vector<int>& a) {
        int n = a.size();
        int l = 0, r = n-1;
        int leftMax = 0, rightMax = 0, ans = 0;
        while(l <= r) {
            if(a[l] < a[r]) {
                rightMax = max(rightMax, a[r]);
                ans += max(0, min(leftMax, rightMax) - a[l]);
                leftMax = max(a[l], leftMax);
                ++l;
            } else {
                leftMax = max(leftMax, a[l]);
                ans += max(0, min(leftMax, rightMax) - a[r]);
                rightMax = max(rightMax, a[r]);
                --r;
            }
        }
        return ans;
    }
};
```
____________
### Non Overlapping Intervals
https://leetcode.com/problems/non-overlapping-intervals/description/

```
Given an array of intervals intervals where intervals[i] = [starti, endi], 
return the minimum number of intervals you need to remove to make 
the rest of the intervals non-overlapping.

Note that intervals which only touch at a point are non-overlapping. 
For example, [1, 2] and [2, 3] are non-overlapping.
```

Actually, the problem is the same as "Given a collection of intervals, find the maximum number of intervals that are non-overlapping." (the classic Greedy problem: Interval Scheduling). With the solution to that problem, guess how do we get the minimum number of intervals to remove? : )

Sorting Interval.end in ascending order is O(nlogn), then traverse intervals array to get the maximum number of non-overlapping intervals is O(n). Total is O(nlogn).

```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& a) {
        /* Need to sort by the end time because 
        the fastest finish time first would make space for the upcoming pairs */
        sort(a.begin(), a.end(), [&](const vector<int> a, const vector<int> b) -> bool {
            return a[1] < b[1];
        });
        int n = a.size();
        vector<int> prv = a[0];
        int cnt = 0;
        for(int i=1; i<n; ++i) {
            // overlapping
            if(a[i][0] < prv[1]) {
                cnt++;
            } else {
                prv = a[i];
            }
        }
        return cnt;
    }
};
```

__________
### LC 287. Find the Duplicate Number

Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and using only constant extra space.

Example 1:

Input: nums = [1,3,4,2,2],
Output: 2

    /*
        Using Floyd Cycle detection
        TC: O(N), SC: O(1)
        Assume each nums value as an address just like in linked list node addr.
        Then since there is one number whith duplicates, that means there are multiple instances
        of the same address, so it is a cycle just like in linked list. Do the same thing
        as in linked list.
    */

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& a) {
        int n = a.size();
        int s = a[0];
        int f = a[s];
        while(f != s) {
            s = a[s];
            f = a[a[f]];
        }
        // s == f means the cycle is detected at point s
        // now need to find the start of the cycle, which should be equal steps from f/s to start;
        s = 0;
        while(s != f) {
            s = a[s];
            f = a[f];
        }
        return f;
    }
};
```
_____________