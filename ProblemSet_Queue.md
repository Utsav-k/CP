### LC 239. Sliding Window Maximum
https://leetcode.com/problems/sliding-window-maximum/description/



You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.
Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]


#### Solution
The idea is to maintain a monotonically increasing queue and the max is at the back.


In the case above the queue would be

1

3

-1 3

-3 -1 3

5

3 5

6

7


```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& a, int k) {
        int n = a.size(), i = 0;
        deque<int> dq;
        vector<int> ans;
        while(i < n) {
            // Reject the index that are expired.
            while(!dq.empty() && dq.back() <= i-k) {
                dq.pop_back();
            }
            while(!dq.empty() && a[dq.front()] < a[i]) {
                dq.pop_front();
            }
            dq.push_front(i);
            if(i >= k-1) ans.push_back(a[dq.back()]);
            i++; 
        }
        return ans;
    }
};
```

_____________

