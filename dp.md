### 200 · Longest Palindromic Substring  
初始化：p[i][i] = True  p[i][i+1] = s[i] == s[i+1]  
转移方程：p[i][j] = p[i+1][j-1] and (p[i] == p[j])  
更新条件：按对角线，向右上角，每次更新**长度相同**的子串是否是回文串

i:行，j:列      
--a b c f c b a    
a 1 0 0 0 0 0 1  
b - 1 0 0 0 1 0  
c - - -1 0 1 0 0  
f - - - - 1 0 0 0  
c - - - - - 1 0 0  
b - - - - - - 1 0  
a - - - - - -  --1    

```python 
class Solution:
    """
    @param s: input string
    @return: a string as the longest palindromic substring
    """
    def longestPalindrome(self, s):
        if not s:
            return ''
        if len(s) == 1:
            return s
        pa = [[False]*len(s) for _ in range(len(s))]
        #init
        longest = 1
        idx = (0,0)
        for i in range(len(s)):
            pa[i][i] = True
            if i < len(s)-1:
                pa[i][i+1] = (s[i] == s[i+1])
                if (s[i] == s[i+1]):
                    longest = 2
                    idx = (i,i+1)
        # print(pa)
        for length in range(2,len(s)+1):
            for start in range(len(s)-length):
                end = start + length
                if_p = pa[start+1][end-1] and (s[start] == s[end])
                # print((start,end))
                # print(if_p)
                pa[start][end] = if_p
                if if_p and (end-start+1 > longest):
                    longest = end-start+1
                    idx = (start,end)
        return s[idx[0]:idx[1]+1]
```
