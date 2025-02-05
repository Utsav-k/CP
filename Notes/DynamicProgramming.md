# Dynamic Programming

## Resources

https://leetcode.com/discuss/general-discussion/458695/Dynamic-Programming-Patterns

https://leetcode.com/discuss/general-discussion/662866/DP-for-Beginners-Problems-or-Patterns-or-Sample-Solutions

https://leetcode.com/discuss/general-discussion/1050391/Must-do-Dynamic-programming-Problems-Category-wise


<<<<<<< HEAD

## Problems (Category Wise)

### Knapsack (0/1)

Subset Sum

Equal Sum Partition

Count of subset sum

Minimum Subset Sum Diff

Target Sum

Number of Subsets with Given Difference

### Unbounded Knapsack

[Coin Change](https://leetcode.com/problems/coin-change/) (Classic Problem)

[Coin Change II](https://leetcode.com/problems/coin-change-2/) 
(Read about why the coins should be in outer loop)

```cpp
int change(int amount, vector<int>& coins) {
    vector<int> dp(amount+1, 0);
    dp[0] = 1;
    for(int &x:coins) {
        for(int i=x; i<=amount; ++i) {
            dp[i] += dp[i-x];
        }
    }
    return dp[amount];
}
```

[CombinationSum4](https://leetcode.com/problems/combination-sum-iv/description/) 
(This is similar to Coing Change 2 but the repititions are allowed, Just reverse the loop)
```cpp
int combinationSum4(vector<int>& A, int target) {
    int n = A.size();
    vector<unsigned int> dp(target+1, 0);
    dp[0] = 1;
    for(int i=1; i<=target; i++) {
        for(int &x:A) {
            if(i >= x) dp[i] += dp[i-x];
        }
    }
    return dp[target];
}
```

[Minimum Cost for Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/) 
(It has the added complexity due to days not being continuous)

```cpp
class Solution {
    const int inf = 1e9+7;
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        unordered_set<int> st(days.begin(), days.end());
        vector<int> dp(367, inf);
        dp[0] = 0;
        for(int i=1; i<=365; ++i) {
            if(st.find(i) == st.end()) {
                // This is important, if you don't have to visit a day, the number remains same.
                dp[i] = dp[i-1];
            } else {
                dp[i] = min(dp[i], dp[i-1] + costs[0]);
                dp[i] = min(dp[i], dp[max(i-7, 0)] + costs[1]);
                dp[i] = min(dp[i], dp[max(i-30, 0)] + costs[2]);
            }
        }
        return dp[days.back()];
    }
};
```

[Rod Cutting Problem](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/) 
(Classic DP Problem, Similar Solution to Coin Change 2)
```cpp
int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for(int i=0; i<n; i++) {
        cin >> a[i];
    }

    vector<int> dp(n+1, 0);
    for(int j=0; j<n; j++) {
        for(int i=j+1; i<=n; ++i) {
            dp[i] = max(dp[i], dp[i-j-1] + a[j]);
        }
    }
    cout << dp[n] << " ";
    return 0;
}
```
____________________________

### Merging Intervals


[Min Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)


__________________________

### Min-Max Kind of Problems

LC64(Classic DP) - https://leetcode.com/problems/minimum-path-sum/description

LC746(Classic DP) - https://leetcode.com/problems/min-cost-climbing-stairs/description/

LC931 - https://leetcode.com/problems/minimum-falling-path-sum/description/

LC322(Classic DP) - https://leetcode.com/problems/coin-change/description/


LC650 - https://leetcode.com/problems/2-keys-keyboard/description/  (Top Down is easy, Bottom up can be tricky. Read the LC article for all solutions)

<<<<<<< HEAD
LC983 - https://leetcode.com/problems/minimum-cost-for-tickets/description/


=======
LC120 - https://leetcode.com/problems/triangle/description/

LC279 - https://leetcode.com/problems/perfect-squares/description (Coin Change kind of problem)


LC983 - https://leetcode.com/problems/minimum-cost-for-tickets/description/


___________


### Longest Common Subsequence

[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description)

Classic DP problem

```cpp
int longestCommonSubsequence(string s1, string s2) {
    int m = s1.length();
    int n = s2.length();
    vector<vector<int>> dp(m+1, vector<int> (n+1));
    // state dp[i][j] means longest subsequence before i in s1 and j in s2;

    for(int i=1; i<=m; ++i) {
        for(int j=1; j<=n; j++) {
            if(s1[i-1] == s2[j-1]) {
                dp[i][j] = 1 + dp[i-1][j-1];
            } else {
                // Do not have to consider dp[i-1][j-1] here as it's taken in account.
                dp[i][j] = max(dp[i-1][j-1], max(dp[i-1][j], dp[i][j-1]));
            }
        }
    }
    return dp[m][n];
}
```

[72. Edit Distance](https://leetcode.com/problems/edit-distance/description/)

Another Classic Variant of Longest Common Subsequence

```cpp
int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1));
        // prepare base cases
        for(int i=1; i<=m; i++) {
            dp[i][0] = i;
        }
        for(int i=1; i<=n; i++) {
            dp[0][i] = i;
        }
        for(int i=1; i<=m; ++i) {
            for(int j=1; j<=n; ++j) {
                if(word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                }
                else {
                    // Here the {i-1, j-1} should be considered because of replace action.
                    dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
                }
            }
        }
        return dp[m][n];
    }
```
