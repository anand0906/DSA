1.Longest common subsequence
Given two strings str1 and str2, find the length of their longest common subsequence.
A subsequence is a sequence that appears in the same relative order but not necessarily contiguous and a common subsequence of 
two strings is a subsequence that is common to both strings.

Examples:
Input: str1 = "bdefg", str2 = "bfg"
Output: 3
Explanation: The longest common subsequence is "bfg", which has a length of 3.

Input: str1 = "mnop", str2 = "mnq"
Output: 2
Explanation: The longest common subsequence is "mn", which has a length of 2.


def solve(s1,s2,index1,index2):
    if(index1<0 or index2<0):
        return 0

    if(s1[index1]==s2[index2]):
        return 1+solve(s1,s2,index1-1,index2-1)
    else:
        exclude1=solve(s1,s2,index1-1,index2)
        exclude2=solve(s1,s2,index1,index2-1)
        return max(exclude1,exclude2)

def solve_memo(s1,s2,index1,index2,memo):
    if(index1<0 or index2<0):
        return 0
    if(memo[index1][index2]!=None):
        return memo[index1][index2]
    if(s1[index1]==s2[index2]):
        memo[index1][index2]=1+solve_memo(s1,s2,index1-1,index2-1,memo)
    else:
        exclude1=solve_memo(s1,s2,index1-1,index2,memo)
        exclude2=solve_memo(s1,s2,index1,index2-1,memo)
        memo[index1][index2]=max(exclude1,exclude2)
    return memo[index1][index2]

def tabulation(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=[[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0]=0
    for index2 in range(n2+1):
        dp[0][index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp[index1][index2]=1+dp[index1-1][index2-1]
            else:
                exclude1=dp[index1-1][index2]
                exclude2=dp[index1][index2-1]
                dp[index1][index2]=max(exclude1,exclude2)
    return dp[n1][n2]

def optimized(s1,s2):
    n1,n2=len(s1),len(s2)
    dp_curr=[None]*(n2+1)
    dp_prev=[None]*(n2+1)

    dp_curr[0]=0
    dp_prev[0]=0
    for index2 in range(n2+1):
        dp_prev[index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp_curr[index2]=1+dp_prev[index2-1]
            else:
                exclude1=dp_prev[index2]
                exclude2=dp_curr[index2-1]
                dp_curr[index2]=max(exclude1,exclude2)
        dp_prev=dp_curr.copy()
    return dp_prev[n2]

s1,s2="bdefg","bfg"
n1,n2=len(s1),len(s2)
print(solve(s1,s2,n1-1,n2-1))
memo=[[None]*n2 for _ in range(n1)]
print(solve_memo(s1,s2,n1-1,n2-1,memo))
print(tabulation(s1,s2))
print(optimized(s1,s2))


---

2.Print Longest Common Subsequence

Print Longest Common Subsequence

Problem Description: Given two strings str1 and str2, print the longest common subsequence of the two strings.

A subsequence of a string is a list of characters of the string where zero or more characters are deleted and they should be in the same order in the subsequence as in the original string.

Input: str1 = "abcd", str2="bdef"
Output: "bd"
Explanation: LCS of two strings is "bd".

Input: str1 = "apple" str2 = "waffle"
Output: "ale" 
Explanation: LCS of two strings is "ale".

def tabulation(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=[[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0]=0
    for index2 in range(n2+1):
        dp[0][index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp[index1][index2]=1+dp[index1-1][index2-1]
            else:
                exclude1=dp[index1-1][index2]
                exclude2=dp[index1][index2-1]
                dp[index1][index2]=max(exclude1,exclude2)
    return dp

def solve(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=tabulation(s1,s2)
    ans=[]
    index1,index2=n1,n2
    while(index1>0 and index2>0):
        if(s1[index1-1]==s2[index2-1]):
            ans.append(s1[index1-1])
            index1-=1
            index2-=1
        else:
            if(dp[index1-1][index2]>dp[index1][index2-1]):
                index1-=1
            else:
                index2-=1
    ans="".join(ans[::-1])
    return ans

s1,s2="bdefg","bfg"
print(solve(s1,s2))

---

3.Longest common substring
Given two strings str1 and str2, find the length of their longest common substring.



A substring is a contiguous sequence of characters within a string.


Examples:
Input: str1 = "abcde", str2 = "abfce"

Output: 2

Explanation: The longest common substring is "ab", which has a length of 2.

Input: str1 = "abcdxyz", str2 = "xyzabcd"

Output: 4

Explanation: The longest common substring is "abcd", which has a length of 4.



def tabulation(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=[[None]*(n2+1) for _ in range(n1+1)]
    for index1 in range(n1+1):
        dp[index1][0]=0
    for index2 in range(n2+1):
        dp[0][index2]=0
    ans=0
    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp[index1][index2]=1+dp[index1-1][index2-1]
                ans=max(ans,dp[index1][index2])
            else:
                dp[index1][index2]=0
    return ans

def optimized(s1,s2):
    n1,n2=len(s1),len(s2)
    dp_curr=[None]*(n2+1)
    dp_prev=[None]*(n2+1)

    dp_curr[0]=0
    dp_prev[0]=0
    for index2 in range(n2+1):
        dp_prev[index2]=0
    ans=0
    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp_curr[index2]=1+dp_prev[index2-1]
                ans=max(ans,dp_curr[index2])
            else:
                dp_curr[index2]=0
        dp_prev=dp_curr.copy()
    return ans

s1,s2="bdefg","bfg"
print(tabulation(s1,s2))
print(optimized(s1,s2))

---

4.Longest palindromic subsequence

Given a string, Find the longest palindromic subsequence length in given string.
A palindrome is a sequence that reads the same backwards as forward.
A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

Examples:
Input: s = "eeeme"

Output: 4

Explanation: The longest palindromic subsequence is "eeee", which has a length of 4.

Input: s = "annb"

Output: 2

Explanation: The longest palindromic subsequence is "nn", which has a length of 2.


def optimized(s1,s2):
    n1,n2=len(s1),len(s2)
    dp_curr=[None]*(n2+1)
    dp_prev=[None]*(n2+1)

    dp_curr[0]=0
    dp_prev[0]=0
    for index2 in range(n2+1):
        dp_prev[index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp_curr[index2]=1+dp_prev[index2-1]
            else:
                exclude1=dp_prev[index2]
                exclude2=dp_curr[index2-1]
                dp_curr[index2]=max(exclude1,exclude2)
        dp_prev=dp_curr.copy()
    return dp_prev[n2]

def solve(s):
    ans=optimized(s,s[::-1])
    return ans

print(solve("eeeme"))
    

---

5.Minimum insertions to make string palindrome

Given a string s, find the minimum number of insertions needed to make it a palindrome. A palindrome is a sequence that reads the same backward as forward. You can insert characters at any position in the string.


Examples:
Input: s = "abcaa"



Output: 2



Explanation: Insert 2 characters "c", and "b" to make "abcacba", which is a palindrome.

Input: s = "ba"



Output: 1



Explanation: Insert "a" at the beginning to make "aba", which is a palindrome.


def optimized(s1,s2):
    n1,n2=len(s1),len(s2)
    dp_curr=[None]*(n2+1)
    dp_prev=[None]*(n2+1)

    dp_curr[0]=0
    dp_prev[0]=0
    for index2 in range(n2+1):
        dp_prev[index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp_curr[index2]=1+dp_prev[index2-1]
            else:
                exclude1=dp_prev[index2]
                exclude2=dp_curr[index2-1]
                dp_curr[index2]=max(exclude1,exclude2)
        dp_prev=dp_curr.copy()
    return dp_prev[n2]

def solve(s):
    lcsLength=optimized(s,s[::-1])
    ans=len(s)-lcsLength
    return ans

---

6.Shortest common supersequence
Given two strings str1 and str2, find the shortest common supersequence.
The shortest common supersequence is the shortest string that contains both str1 and str2 as subsequences.


Examples:
Input: str1 = "mno", str2 = "nop"

Output: "mnop"

Explanation: The shortest common supersequence is "mnop". It contains "mno" as the first three characters and "nop" as the last three characters, thus including both strings as subsequences.

Input: str1 = "dynamic", str2 = "program"

Output: "dynprogramic"

Explanation: The shortest common supersequence is "dynprogramic". It includes all characters from both "dynamic" and "program", with minimal overlap. 
For example, "dynamic" appears as "dyn...amic" and "program" appears as "...program..." within "dynprogramic".



def tabulation(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=[[None]*(n2+1) for _ in range(n1+1)]
    
    for index1 in range(n1+1):
        dp[index1][0]=0
        
    for index2 in range(n2+1):
        dp[0][index2]=0

    for index1 in range(1,n1+1):
        for index2 in range(1,n2+1):
            if(s1[index1-1]==s2[index2-1]):
                dp[index1][index2]=1+dp[index1-1][index2-1]
            else:
                exclude1=dp[index1-1][index2]
                exclude2=dp[index1][index2-1]
                dp[index1][index2]=max(exclude1,exclude2)
    return dp

def solve(s1,s2):
    n1,n2=len(s1),len(s2)
    dp=tabulation(s1,s2)
    index1,index2=n1,n2
    ans=[]
    while index1>0 and index2>0:
        if(s1[index1-1]==s2[index2-1]):
            ans.append(s1[index1-1])
            index1-=1
            index2-=1
        else:
            if(dp[index1-1][index2]>dp[index1][index2-1]):
                ans.append(s1[index1-1])
                index1-=1
            else:
                ans.append(s2[index2-1])
                index2-=1
    while index1>0:
        ans.append(s1[index1-1])
        index1-=1
    while index2>0:
        ans.append(s2[index2-1])
        index2-=1
    ans="".join(ans[::-1])
    return ans

print(solve("brute","groot"))
                

