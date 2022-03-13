给你一个字符串 s，找到 s 中最长的回文子串。

 

示例 1：

输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
示例 2：

输入：s = "cbbd"
输出："bb"



```C++
class Solution {
public:
    pair<int, int> getPalindrome(const string& s, int start, int end)
    {
        while(start >= 0 && end < s.length() && s[start] == s[end])
        {
            start--;
            end++;
        }
        return {start+1, end-1};
    }

    string longestPalindrome(string s) {
        if(s.length() < 2)  return s;
        int left = 0, len = 0;

        // 中心扩散
        for(int i=0; i<s.length(); i++)
        {
            auto [l1, r1] = getPalindrome(s, i, i);
            auto [l2, r2] = getPalindrome(s, i, i+1);
            if(r1 - l1 + 1 > len)
            {
                left = l1;
                len = r1 - l1 + 1;
            }
            if(r2 - l2 + 1 > len)
            {
                left = l2;
                len = r2 - l2 + 1;
            }
        }
        return s.substr(left, len);
    }
    
    string longestPalindrome(string s)
    {
        if(s.length() < 2)  return s;
        int left = 0, len = 0;

        vector<vector<bool>> dp(s.length(), vector<bool>(s.length(), false));
        // 枚举回文子串长度
        
        for(int i=0; i<s.length(); i++) dp[i][i] = true;

        for(int L=2; L<=s.length(); L++)  // 枚举回文子串的长度
        {
            for(int i=0; i<s.length(); i++)
            {
                int j = L + i - 1;  // 右边界
                if(j >= s.length())     break;

                if(s[j] == s[i])
                {
                    if(j - i <= 2)
                        dp[i][j] = true;
                    else
                        dp[i][j] = dp[i+1][j-1];
                }
                // 不等则非回文子串
                if(dp[i][j] && L > len)
                {
                    len = L;
                    left = i;
                }
            }
        }
        if(len == 0)    len++;
        return s.substr(left, len);
    }
};
```

