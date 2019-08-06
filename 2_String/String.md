## **String**

[TOC]

### 1. KMP和BM算法

谈到字符串问题，不得不提的就是 KMP 算法，它是用来解决字符串查找的问题，可以在一个字符串（S）中查找一个子串（W）出现的位置。KMP 算法把字符匹配的时间复杂度缩小到 O(m+n) ,而空间复杂度也只有O(m)。因为“暴力搜索”的方法会反复回溯主串，导致效率低下，而KMP算法可以利用已经部分匹配这个有效信息，保持主串上的指针不回溯，通过修改子串的指针，让模式串尽量地移动到有效的位置。

具体算法细节请参考：

- [字符串匹配的KMP算法](http://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html)
- [从头到尾彻底理解KMP](https://blog.csdn.net/v_july_v/article/details/7041827)
- [如何更好的理解和掌握 KMP 算法?](https://www.zhihu.com/question/21923021)
- [KMP 算法详细解析](https://blog.sengxian.com/algorithms/kmp)
- [图解 KMP 算法](http://blog.jobbole.com/76611/)
- [汪都能听懂的KMP字符串匹配算法【双语字幕】](https://www.bilibili.com/video/av3246487/?from=search&seid=17173603269940723925)
- [KMP字符串匹配算法1](https://www.bilibili.com/video/av11866460?from=search&seid=12730654434238709250)

BM算法也是一种精确字符串匹配算法，它采用从右向左比较的方法，同时应用到了两种启发式规则，即坏字符规则 和好后缀规则 ，来决定向右跳跃的距离。基本思路就是从右往左进行字符匹配，遇到不匹配的字符后从坏字符表和好后缀表找一个最大的右移值，将模式串右移继续匹配。



### 2. 替换空格

**题目**：请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

**分析**：



**代码**：

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        StringBuffer res = new StringBuffer();
        int len = str.length() - 1;
        for(int i = len; i >= 0; i--){
            if(str.charAt(i) == ' ')
                res.append("02%");
            else
                res.append(str.charAt(i));
        }
        return res.reverse().toString();
    }
}
```



### 3. 打印字符串的全排列

**题目**：输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

注意：输入的字符串的长度不超过9(可能有字符重复),字符只包括大小写字母。

**分析**：把问题拆解成简单的步骤：第一步求所有可能出现在第一个位置的字符（即把第一个字符和后面的所有字符交换[相同字符不交换]）；第二步固定第一个字符，求后面所有字符的排列。这时候又可以把后面的所有字符拆成两部分（第一个字符以及剩下的所有字符），依此类推。这样，我们就可以用递归的方法来解决。

**代码**：

```java
public class Solution {
    ArrayList<String> res = new ArrayList<String>();
    public ArrayList<String> Permutation(String str) {
        if(str == null)
            return res;
        PermutationHelper(str.toCharArray(), 0);
        Collections.sort(res);
        return res;
    }
    public void PermutationHelper(char[] str, int i){
        if(i == str.length - 1){
            res.add(String.valueOf(str));
        }else{
            for(int j = i; j < str.length; j++){
                if(j!=i && str[i] == str[j])
                    continue;
                swap(str, i, j);
                PermutationHelper(str, i+1);
                swap(str, i, j);
            }
        }
    }
    public void swap(char[] str, int i, int j) {
        char temp = str[i];
        str[i] = str[j];
        str[j] = temp;
    }
}
```



### 4. 第一个只出现一次的字符

**题目**：在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

**分析**：先在hash表中统计各字母出现次数，第二次扫描直接访问hash表获得次数。也可以用数组代替hash表。

**代码**：

```java
import java.util.HashMap;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        int len = str.length();
        if(len == 0)
            return -1;
        HashMap<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < len; i++){
            if(map.containsKey(str.charAt(i))){
                int value = map.get(str.charAt(i));
                map.put(str.charAt(i), value+1);
            }else{
                map.put(str.charAt(i), 1);
            }
        }
        for(int i = 0; i < len; i++){
            if(map.get(str.charAt(i)) == 1)
                return i;
        }
        return -1;
    }
}
```



### 5. 左旋转字符串

**题目**：汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。

**分析**：在第 n 个字符后面将切一刀，将字符串分为两部分，再重新并接起来即可。注意字符串长度为 0 的情况。

**代码**：

```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        int len = str.length();
        if(len == 0)
            return "";
        n = n % len;
        String s1 = str.substring(n, len);
        String s2 = str.substring(0, n);
        return s1+s2;
    }
}
```



### 6. 把字符串转换成整数

**题目**：将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

注意：输入的字符串,包括数字字母符号,可以为空。

示例 1：输入：‘+2147483647’，输出：2147483647。

示例 2：输入：‘1a33’，输出：0。

**分析**：



**代码**：

```java
public class Solution {
    public int StrToInt(String str) {
        if(str.length() == 0)
            return 0;
        int flag = 0;
        if(str.charAt(0) == '+')
            flag = 1;
        else if(str.charAt(0) == '-')
            flag = 2;
        int start = flag > 0 ? 1 : 0;
        long res = 0;
        while(start < str.length()){
            if(str.charAt(start) > '9' || str.charAt(start) < '0')
                return 0;
            res = res * 10 + (str.charAt(start) - '0');
            start ++;
        }
        return flag == 2 ? -(int)res : (int)res;
    }
}
```



### 7. 正则表达式匹配

**题目**：请实现一个函数用来匹配包括'.'和'\*'的正则表达式。模式中的字符'.'表示任意一个字符，而'\*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab\*ac\*a"匹配，但是与"aa.a"和"ab*a"均不匹配。

示例 1：输入：‘aa’ 和'a'，输出：false。解释：‘a'无法匹配’aa‘整个字符串。

示例 2：输入：‘aa’ 和'a\*'，输出：true。解释：‘a\*'可以匹配’aa‘整个字符串。

示例 2：输入：‘ab’ 和'.\*'，输出：true。解释：".\*"表示可以匹配零个或多个('\*')任意字符('.')。

**分析**：动态规划：采用dp[i+1] [j+1]代表s[0..i]匹配p[0..j]的结果，结果自然是采用布尔值True/False来表示。首先，对边界进行赋值，显然dp[0] [0] = true，两个空字符串的匹配结果自然为True;接着，对dp[0] [j+1]进行赋值，因为 i=0 是空串，如果一个空串和一个匹配串想要匹配成功，那么只可能是p.charAt(j) == ‘*’ && dp[0] [j-1]。

**代码**：

```java
public boolean isMatch(String s, String p) {
    if (s == null || p == null) {
        return false;
    }
    boolean[][] dp = new boolean[s.length()+1][p.length()+1];
    dp[0][0] = true;
    for (int j = 0; i < p.length(); j++) {
        if (p.charAt(j) == '*' && dp[0][j-1]) {
            dp[0][j+1] = true;
        }
    }
    for (int i = 0 ; i < s.length(); i++) {
        for (int j = 0; j < p.length(); j++) {
            if (p.charAt(j) == '.') {
                dp[i+1][j+1] = dp[i][j];
            }
            if (p.charAt(j) == s.charAt(i)) {
                dp[i+1][j+1] = dp[i][j];
            }
            if (p.charAt(j) == '*') {
                if (p.charAt(j-1) != s.charAt(i) && p.charAt(j-1) != '.') {
                    dp[i+1][j+1] = dp[i+1][j-1];
                } else {
                    dp[i+1][j+1] = (dp[i+1][j] || dp[i][j+1] || dp[i+1][j-1]);
                }
            }
        }
    }
    return dp[s.length()][p.length()];
}
```



### 8. 表示数值的字符串

**题目**：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

**分析**：设置三个标志符分别记录“+/-”、“e/E”和“.”是否出现过。

**代码**：

```java
public class Solution {
    public boolean isNumeric(char[] str) {
        int len = str.length;
        boolean sign = false, decimal = false, hasE = false;
        for(int i = 0; i < len; i++){
            if(str[i] == '+' || str[i] == '-'){
                if(!sign && i > 0 && str[i-1] != 'e' && str[i-1] != 'E')
                    return false;
                if(sign && str[i-1] != 'e' && str[i-1] != 'E')
                    return false;
                sign = true;
            }else if(str[i] == 'e' || str[i] == 'E'){
                if(i == len - 1)
                    return false;
                if(hasE)
                    return false;
                hasE = true;
            }else if(str[i] == '.'){
                if(hasE || decimal)
                    return false;
                decimal = true;
            }else if(str[i] < '0' || str[i] > '9')
                return false;
        }
        return true;
    }
}
```



### 9. 字符流中第一个不重复的字符

**题目**：请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。注意：如果当前字符流没有存在出现一次的字符，返回#字符。

**分析**：用一个哈希表来存储每个字符及其出现的次数，另外用一个字符串 s 来保存字符流中字符的顺序。

**代码**：

```java
import java.util.HashMap;
public class Solution {
    HashMap<Character, Integer> map = new HashMap<Character, Integer>();
    StringBuffer s = new StringBuffer();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        s.append(ch);
        if(map.containsKey(ch)){
            map.put(ch, map.get(ch)+1);
        }else{
            map.put(ch, 1);
        }
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        for(int i = 0; i < s.length(); i++){
            if(map.get(s.charAt(i)) == 1)
                return s.charAt(i);
        }
        return '#';
    }
}
```

### 10. [Rotate String](https://leetcode.com/problems/rotate-string/) [easy]

**题目**：给定两个字符串, A 和 B。A 的旋转操作就是将 A 最左边的字符移动到最右边。 例如, 若 A = 'abcde'，在移动一次之后结果就是'bcdea' 。如果在若干次旋转操作之后，A 能变成B，那么返回True。

示例 1: 输入: A = 'abcde', B = 'cdeab'，输出: true。

示例 2: 输入: A = 'abcde', B = 'abced'，输出: false。

注意：A 和 B 长度不超过 100。

**分析**：



**代码**：

```java
class Solution {
    public boolean rotateString(String A, String B) {
        return A.length() == B.length() && (A+A).contains(B);
    }
}
```



### 11. [Reverse String]() [easy]

**题目**：编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

示例 1：输入：["h","e","l","l","o"]，输出：["o","l","l","e","h"]。

示例 2：输入：["H","a","n","n","a","h"]，输出：["h","a","n","n","a","H"]。

**分析**：



**代码**：

```java
class Solution {
    public String reverseString(String s) {
        if(s.length() < 2)
            return s;
        int l = 0, r = s.length() - 1;
        char [] strs = s.toCharArray(); 
        while(l < r){
            char temp = strs[l];
            strs[l] = strs[r];
            strs[r] = temp;
            l++;
            r--;
        }
        return new String(strs);
    }
}
```



### 12. [Valid Anagram](https://leetcode.com/problems/valid-anagram) [easy]

**题目**：给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1: 输入: s = "anagram", t = "nagaram"，输出: true。

示例 2: 输入: s = "rat", t = "car"，输出: false。

说明: 

1. 你可以假设字符串只包含小写字母。
2. 进一步思考，如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

**分析**：



**代码**：



### 13. [Group Anagrams](https://leetcode.com/problems/group-anagrams/) [medium]

**题目**：给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例: 输入: ["eat", "tea", "tan", "ate", "nat", "bat"]，输出:[ ["ate","eat","tea"], ["nat","tan"], ["bat"]]。

说明：1. 所有输入均为小写字母。2. 不考虑答案输出的顺序。

**分析**：



**代码**：

### 14. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses) [easy]

**题目**：给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。有效字符串需满足: 

1.左括号必须用相同类型的右括号闭合。2.左括号必须以正确的顺序闭合。

注意:空字符串可被认为是有效字符串。

示例 1: 输入: "()"，输出: true。

示例 2: 输入: "()[]{}"，输出: true。

示例 3: 输入: "(]"，输出: false。

示例 4: 输入: "([)]"，输出: false。

示例 5: 输入: "{[]}"，输出: true。

**分析**：



**代码**：





### 15. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/) [easy]

**题目**：给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1: 输入: "A man, a plan, a canal: Panama"，输出: true。

示例 2: 输入: "race a car"，输出: false。

**分析**：两个指针比较头尾。要注意只考虑字母和数字字符，可以忽略字母的大小写。

**代码**：

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.length() == 0)
             return true;
        int l = 0, r = s.length() - 1;
        while(l < r){
            if(!Character.isLetterOrDigit(s.charAt(l))){
                l++;
            }else if(!Character.isLetterOrDigit(s.charAt(r))){
                r--;
            }else{
                if(Character.toLowerCase(s.charAt(l)) != Character.toLowerCase(s.charAt(r)))
                    return false;
                l++;
                r--;
            } 
        }
        return true;
    }
}
```





### 16. [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/) [easy]

**题目**：给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。

示例 1: 输入: "abc"，输出: 3。解释: 三个回文子串: "a", "b", "c"。

示例 2: 输入: "aaa"，输出: 6。解释: 6个回文子串: "a", "a", "a", "aa", "aa", "aaa"。

注意: 输入的字符串长度不会超过1000。

**分析**：



**代码**：





### 17. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) [easy]

**题目**：编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。注意，所有输入只包含小写字母a-z。

示例 1: 输入: ["flower","flow","flight"]，输出: "fl"。

示例 2: 输入: ["dog","racecar","car"]，输出: ""。解释: 输入不存在公共前缀。

**分析**：首先对字符串数组进行排序，然后拿数组中的第一个和最后一个字符串进行比较，从第 0 位开始，如果相同，把它加入 res 中，不同则退出。最后返回 res。

**代码**：

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs == null || strs.length == 0)
            return "";
        Arrays.sort(strs);
        char [] first = strs[0].toCharArray();
        char [] last = strs[strs.length - 1].toCharArray();
        StringBuffer res = new StringBuffer();
        int len = first.length < last.length ? first.length : last.length;
        int i = 0;
        while(i < len){
            if(first[i] == last[i]){
                res.append(first[i]);
                i++;
            }
            else
                break;
        }
        return res.toString();
    }
}
```



### 18. [Longest Palindrome](https://leetcode.com/problems/longest-palindrome/) [easy]

**题目**：给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。

说明: 输入字符串的长度不会超过 1010。

示例 1: 输入: "abccccdd"，输出: 7。解释: 我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

**分析**：统计字母出现的次数即可，双数才能构成回文。因为允许中间一个数单独出现，比如“abcba”，所以如果最后有字母落单，总长度可以加 1。

**代码**：

```java
class Solution {
    public int longestPalindrome(String s) {
        HashSet<Character> hs = new HashSet<>();
        int len = s.length();
        int count = 0;
        if(len == 0)
            return 0;
        for(int i = 0; i<len; i++){
            if(hs.contains(s.charAt(i))){
                hs.remove(s.charAt(i));
                count++;
            }else{
                hs.add(s.charAt(i));
            }
        }
        return hs.isEmpty() ? count * 2 : count * 2 + 1;
    }
}
```



### 19. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring) [medium]

**题目**：给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：输入: "babad"，输出: "bab"。注意: "aba" 也是一个有效答案。

示例 2：输入: "cbbd"，输出: "bb"。

**分析**：以某个元素为中心，分别计算偶数长度的回文最大长度和奇数长度的回文最大长度。

**代码**：

```java
class Solution {
    private int index, len;
    public String longestPalindrome(String s) {
        if(s.length() < 2)
            return s;
        for(int i = 0; i < s.length()-1; i++){
            PalindromeHelper(s, i, i);
            PalindromeHelper(s, i, i+1);
        }
        return s.substring(index, index+len);
    }
    public void PalindromeHelper(String s, int l, int r){
        while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)){
            l--;
            r++;
        }
        if(len < r - l - 1){
            index = l + 1;
            len = r - l - 1;
        }
    }
}
```



### 20. [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) [medium]

**题目**：给定一个字符串s，找到其中最长的回文子序列。可以假设s的最大长度为1000。

示例 1: 输入: "bbbab"，输出: 4。解释：一个可能的最长回文子序列为 "bbbb"。

示例 2: 输入: "cbbd"，输出: 2。解释：一个可能的最长回文子序列为 "bb"。

**分析**：动态规划

**代码**：

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        int [][] dp = new int[len][len];
        for(int i = len - 1; i>=0; i--){
            dp[i][i] = 1;
            for(int j = i+1; j < len; j++){
                if(s.charAt(i) == s.charAt(j))
                    dp[i][j] = dp[i+1][j-1] + 2;
                else
                    dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
            }
        }
        return dp[0][len-1];
    }
}
```



### 21. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) [medium]

**题目**：给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1: 输入: "abcabcbb"，输出: 3 。解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2: 输入: "bbbbb"，输出: 1。解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3: 输入: "pwwkew"，输出: 3。解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。注意，答案必须是子串的长度，"pwke" 是一个子序列，不是子串。

**分析**：



**代码**：



### 22. [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/) [medium]

**题目**：给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意: 字符串长度和k不会超过 104。

示例 1: 输入: s = "ABAB", k = 2，输出: 4。解释: 用两个'A'替换为两个'B',反之亦然。

示例 2: 输入: s = "AABABBA", k = 1，输出: 4。

解释: 将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。子串 "BBBB" 有最长重复字母, 答案为 4。

**分析**：



**代码**：



### 23. [Permutation in String](https://leetcode.com/problems/permutation-in-string/) [medium] 

**题目**：给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。换句话说，第一个字符串的排列之一是第二个字符串的子串。

注意：输入的字符串只包含小写字母。两个字符串的长度都在 [1, 10,000] 之间

示例1: 输入: s1 = "ab" s2 = "eidbaooo"，输出: True。解释: s2 包含 s1 的排列之一 ("ba").

示例2: 输入: s1= "ab" s2 = "eidboaoo"，输出: False。

**分析**：不用真的去算出s1的全排列，只要统计字符出现的次数即可。可以使用一个哈希表配上双指针来做。



**代码**：

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int l1 = s1.length();
        int l2 = s2.length();
        int [] count = new int [128];
        if(l1 > l2)
            return false;
        for(int i = 0; i<l1; i++){
            count[s1.charAt(i) - 'a']++;
            count[s2.charAt(i) - 'a']--;
        }
        if(allZero(count))
            return true;
        for(int i = l1; i<l2; i++){
            count[s2.charAt(i) - 'a']--;
            count[s2.charAt(i-l1) - 'a']++;
            if(allZero(count))
                return true;
        }
        return false;
    }
    public boolean allZero(int [] count){
        int l = count.length;
        for(int i = 0; i < l; i++){
            if(count[i] != 0)
                return false;
        }
        return true;
    }
}
```

### 24. [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/) [medium]

**题目**：给定一个字符串，逐个翻转字符串中的每个单词。

示例 1：输入: "the sky is blue"，输出: "blue is sky the"。

示例 2：输入: "  hello world!  "，输出: "world! hello"。

解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

示例 3：输入: "a good   example"，输出: "example good a"。

解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

说明：

1. 无空格字符构成一个单词。
2. 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
   如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
3. 进一步的，如果使用C语言，请思考 O(1) 额外空间复杂度的原地解法。

**分析**：借助trim()和 split()

**代码**：

```java
public class Solution {
    public String reverseWords(String s) {
        if(s.trim().length() == 0)
            return s.trim();
        String [] temp = s.trim().split(" +");
        String res = "";
        for(int i = temp.length - 1; i > 0; i--){
            res += temp[i] + " ";
        }
        return res + temp[0];

    }
}
```



### 25. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/) [hard]

**题目**：给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字母的最小子串。

说明：

1. 如果 S 中不存这样的子串，则返回空字符串 ""。
2. 如果 S 中存在这样的子串，我们保证它是唯一的答案。

示例: 输入: S = "ADOBECODEBANC", T = "ABC"，输出: "BANC"

**分析**：



**代码**：







### 26. Reference

- https://www.weiweiblog.cn/13string/
- https://leetcode.com