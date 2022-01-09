## [541. 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

#### self

```
class Solution {
public:
    string reverseStr(string  & s, int k) 
    {
        int q = s.length() / (2*k);
        int r = s.length() % (2*k);
        string temp = s;
        for(int i = 0; i < q; i++)
        {
            for(int j = 0; j < k; j++)
            {
                temp[i*2*k + j] = s[i*2*k + k - j - 1];
            }
        }
        int num =;
        if(r >= k)
            num = k;
        else
            num = r;
        for(int i = 0; i<num; i++)
        {
            temp[q*2*k + i] = s[q*2*k + num- i - 1];
        }
        return temp;
    }
};
```

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]
示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        auto N = nums.size();
        int rem = 0;
        vector<int> res;
        for(int i = 0; i<N; i++)
        {
            rem = target - nums[i];
            for(int j = i + 1; j<N; j++)
            {
                if(rem == nums[j])
                {
                    res.push_back(i);
                    res.push_back(j);
                    return res;
                }
            }
        }
        return res;
    }

};
```

排序后再查找会不会更快？不建议，应该使用map

#### 两遍哈希表

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> nums_map;
        vector<int> res;
        int rem = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            // nums_map.insert({nums[i], i});
            nums_map[nums[i]] = i;
        }
        for(int i = 0; i < nums.size() ; i++)
        {
            rem = target - nums[i];
            auto find_itr = nums_map.find(rem);
            if(find_itr != nums_map.cend() && find_itr->second != nums[i])
            {
                res.push_back(i);
                res.push_back(nums_map[rem]);
                break;
            }
        }
        return res;
    }
};
```

set才可使用insert快速初始化一个vector的内容

#### 一遍哈希表**

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> nums_map;
        vector<int> res(2,-1);
        for(int i = 0; i < nums.size(); i++)
        {
            if(nums_map.find(target-nums[i]) != nums_map.cend())
            {
                res[0] = nums_map[target-nums[i]];
                res[1] = i;
            }
            nums_map[nums[i]] = i;
        }
        return res;
    }
};
```

#### 官方

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }
};
```

为何使用++i  执行用时更快？

## [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

##### 获取整数位数

```
int length = 1;
int x = 234567545;
while ( x /= 10 )
   length++;
```

#### 取x的一半比较**

```
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0 || (x % 10 == 0 && x !=0))
        {
            return false;
        }
        int rev = 0;
        while(x > rev)
        {
            rev = rev*10 + x%10;
            x /= 10;
        }
        return x == rev || x == rev/10;
    }
};
```

![image-20220107115227485](E:\研二上\leetcode\README\image\image-20220107115227485.png)？？？

#### 转化字符串

```
class Solution {
public:
    bool isPalindrome(int x) {
        if( x<0 || (x%10 == 0 && x!=0) )
        {
            return false;
        }
        string s = to_string(x);
        return s == string(s.rbegin(), s.rend());
    }
};
```

#### 双指针

```
class Solution {
public:
    bool isPalindrome(int x) {
        if( x<0 || (x%10 == 0 && x!=0) )
        {
            return false;
        }
        string s = to_string(x);
        int left = 0;
        int right = s.length() - 1;
        while(left < right)
        {
            if(s[left] != s[right])
            {
                return false;
            }
            ++left;
            --right;
        }
        return true;
    }
};
```



## [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

#### 从左向右遍历

```
class Solution {
private:
    unordered_map<char,int> RomanMap{
        {'I',1},
        {'V',5},
        {'X',10},
        {'L',50},
        {'C',100},
        {'D',500},
        {'M',1000},
    };
public:
    int romanToInt(string s) {
        int res = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            int val = RomanMap[s[i]];
            if(i<s.length()-1 && val<RomanMap[s[i+1]])
            {
                res -= val;
            }
            else
            {
                res += val;
            }
        }
        return res;
    }
};
```

#### 从右向左遍历**

```
class Solution {
private:
    unordered_map<char,int> RomanMap{
        {'I',1},
        {'V',5},
        {'X',10},
        {'L',50},
        {'C',100},
        {'D',500},
        {'M',1000},
    };
public:
    int romanToInt(string s) {
        int res = 0;
        for(int i = s.length()-1, last_val = 0,  val = 0; i >= 0; --i)
        {
            val = RomanMap[s[i]];
            if(val < last_val)
            {
                res -= val;
            }
            else
            {
                res += val;
            }
            last_val = val;
        }
        return res;
    }
};
```

注意边界判断

## [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

#### 先找出数组中字典序最小和最大的字符串，最长公共前缀即为这两个字符串的公共前缀**

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        // c++17 结构化绑定
        // str0, str1 分别是一个 pair<string, string> 的 first 和 secon
        const auto [str0, str1] = minmax_element(strs.begin(), strs.end());
        for(int i = 0; i < str0 -> size(); ++i)
        {
            if (str0 -> at(i) != str1 -> at(i))
            {
                return str0 -> substr(0, i);
            }
        }
        return *str0;//注意此处不能返回""
    }
};
```

#### 排序后比较

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return string();
        sort(strs.begin(), strs.end());
        string st = strs.front(), en = strs.back();
        int i, num = min(st.size(), en.size());
        for(i = 0; i < num && st[i] == en[i]; i ++);
        return string(st, 0, i);
    }
};
```

#### 横向扫描

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";
        string prefix = strs[0];
        for(int i = 1; i < strs.size(); ++i)
        {
            prefix = compare2str(prefix, strs[i]);
            if(prefix.empty()) break;
        }
        return prefix;
    }
    
    string compare2str(string & str1, string & str2)
    {
        int len = min(str1.size(), str2.size());
        int i = 0;
        while(i < len && str1[i]==str2[i])
        {
            ++i;
        }
        return str1.substr(0, i);
    }
};
```

#### 纵向扫描 **

如何避免越界？

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";
        int len = 0;
        int length = strs[0].size();
        int count = strs.size();
        while(len < length)
        {
            char ch = strs[0][len];
            for(int i = 0; i < count; ++i)
            {
                if(len == strs[i].length() || strs[i][len] != ch) //防越界
                {
                    return strs[0].substr(0, len);
                }
            }
            ++len;
        }
        return strs[0];
    }
};
```

#### 利用find

利用库函数是不是不太好？

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string res = strs.empty()? "" : strs[0];
        /*
        在字符串s中查找res并返回首字母的位置（find函数）
        如果首地址不为零，每次令res-1以缩短公共前缀
        比如说再flow中查找flower，没有找到，返回值为迭代器结尾（非0）
        公共前缀会减掉最后一个字母，为flowe。继续循环直到为flow
        如果是首字母不一样则公共前缀会清空
        */ 
        for(string s : strs)
        {
            while(s.find(res) != 0)
            {
                res = res.substr(0, res.length()-1);
                if(res == "") return res;
            }
        }
        return res;
    }
};
```



## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

#### 栈**

```
class Solution {
private:
    unordered_map<char,char> map{
        {'(', ')'},
        {'{', '}'},
        {'[', ']'},
    };
public:
    bool isValid(string s) {
        if(! s.length() % 2) return false;
        stack<char> stk;
        for(char c : s)
        {
            if(map.count(c))
            {
                stk.push(c);
            }
            else if(!stk.empty() && c == map[stk.top()])
            {
                stk.pop();
            }
            else return false;
        }
        return stk.empty();
    }
};
```

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

#### 递归



#### 迭代head last**

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        // if(list1 == nullptr) return list2;
        // else if (list2 == nullptr) return list1; //not need
        ListNode* head = new ListNode();
        ListNode* last = head;
        while(list1 != nullptr && list2 != nullptr)
        {
            if(list1->val < list2->val)
            {
                last->next = list1;//修改指向节点
                list1 = list1->next;//截短list1
            }
            else
            {
                last->next = list2;
                list2 = list2->next;
            }
            last = last->next;//移动last
        }
        //if(list1 == nullptr) last->next = list2;
        //else last->next = list1;
        last->next = (list1 == nullptr ? list2 : list1);
        return head->next;
    }
};
```

## [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

#### 双指针

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int low = 0;
        for(int fast = 1; fast < nums.size(); ++fast)
        {
            if(nums[low] != nums[fast])
            {
                nums[++low] = nums[fast];
            }
        }
        return low+1;//++low 所以return low+1
    }
};
```

## [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

#### 双指针

遇到val   ++fast

未遇到val  赋值 ++low  ++fast

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int low = 0;
        for(int fast = 0; fast <nums.size(); ++fast)
        {
            if(nums[fast] != val)
            {
                nums[low++] = nums[fast];
            }
        }
        return low;
    }
};
```

#### 双指针优化**

以下两个代码均正确

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int fast = nums.size()-1;
        int slow = 0;
        while(slow <= fast)//<还是<= 与fast是否-1有关//疑问：如何快速正确选取
        {
            if(nums[slow] == val)
            {
                while(nums[fast] == val && fast > 0) --fast;
                nums[slow] = nums[fast--];
                //若出现slow=fast时为val，会多出现一次无效交换，但对low无影响
            }
            else slow++;  
        }
        return slow;
    }
};
```

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int fast = nums.size();
        int slow = 0;
        while(slow < fast)//<还是<= 与fast是否-1有关//疑问：如何快速正确选取
        {
            if(nums[slow] == val)
            {
                while(nums[fast-1] == val && fast > 1) --fast;
                nums[slow] = nums[fast-1];
                --fast;
            }
            else slow++;  
        }
        return slow;
    }
};
```

## [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

#### find

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        //if(haystack.find(needle) == haystack.npos) return -1;
        return (haystack.find(needle));
    }
};
```

#### AC

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        // if(needle == "") return 0;//not need
        int n = haystack.length(), m = needle.length();
        bool res = true;
        for(int i = 0; i+m <= n; ++i)
        {
            res = true;
            for(int j = 0; j < m; ++j)
            {
                 if(haystack[i+j] != needle[j])
                 {
                     res = false;
                     break;
                 } 
            }
            if(res) return i;
        }
        return -1;
    }
};
```

#### KMP算法**

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.length(), m = needle.length();
        if(!m) return 0;
        vector<int> next(m, 0);
        for(int i = 1, j = 0; i < m; ++i)
        {
            while(j > 0 && needle[i] != needle[j]) //先检查j更优
            {
                j = next[j-1];//不相等一直回退j指针
            }
            if(needle[i] == needle[j])
            {
                ++j;
            }
            next[i] = j;//next数组赋值 j已经被加过1所以直接赋值j
        }
        for(int i = 0, j = 0; i < n; ++i)
        {
            while(j > 0 && haystack[i] != needle[j])
            {
                j = next[j-1];//不相等一直回退j指针
            }
            if(haystack[i] == needle[j])
            {
                ++j;
            }
            if(j == m) return (i-m+1);
        }
        return -1;
    }
};
```

##### 视频讲解

https://www.bilibili.com/video/BV18k4y1m7Ar/?spm_id_from=333.788.recommend_more_video.3

05:35 next数组构造讲解

## [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

由于如果存在这个目标值，我们返回的索引也是pos，因此我们可以将两个条件合并得出最后的目标：「在一个有序数组中找第一个大于等于target 的下标」

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size()-1;
        while(left<=right)
        {
            int mid = left + (right - left) >> 2; //该写法防止溢出
            int val = nums[mid];
            if(target > val) left = mid + 1;
            else right = mid - 1; 
        }
        return left;
    }
};
```

![image-20220109204710773](E:\研二上\leetcode\README\image\image-20220109204710773.png)

![image-20220109205627493](E:\研二上\leetcode\README\image\image-20220109205627493.png)

#### 模板化二分法**

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = -1, right = nums.size();//找到第一个>= target的值
        while(left+1 != right)
        {
            int mid = left + (right-left)/2;
            if(nums[mid] < target) left = mid;
            else right = mid; 
        }
        return right;
    }
};
```

## [53. 最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)

#### AC 超出时间限制

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max = INT_MIN;
        int n = nums.size();
        for(int i = 0; i < n; ++i)
        {
            int sum = 0;
            for(int j = i; j < n; ++j)
            {
                sum += nums[j];
                if(sum > max) max = sum;
            }
        }
        return max;
    }
};
```

#### 动态规划O(n)

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int cursum = 0, res = nums[0];
        for(const auto &x : nums)
        {
            cursum = max(cursum+x, x);
            res = max(res, cursum);
        }
        return res;
    }
};
```

#### 贪心算法

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int cursum = 0, res = INT_MIN;
        for(const auto &x : nums)
        {
            cursum += x;
            res = max(res, cursum);
            if(cursum < 0) cursum = 0;
        }
        return res;
    }
};
```

#### 分治 ？

## [58. 最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

#### 反向遍历

```
class Solution {
public:
    int lengthOfLastWord(string s) {
        int i = s.length()-1, len = 0;
        for(; i >= 0; --i)
        {
            if(s[i] != ' ') break;
        }
        for(; i >= 0; --i)
        {
            if(s[i] == ' ') break;
            ++len;
        }
        return len;
    }
};
```

#### while改写

```
class Solution {
public:
    int lengthOfLastWord(string s) {
        int i = s.length()-1, len = 0;
        while(s[i] == ' ') --i;//i不会越界
        while(i >= 0 && s[i--] != ' ') ++len;//注意修改i
        return len;
    }
};
```

## [66. 加一](https://leetcode-cn.com/problems/plus-one/)

#### 递归(self)

```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        plus(digits, digits.size()-1);
        return digits;
    }
    void plus(vector<int>& digits, int pos){
        if(digits[pos] == 9)
        {
            digits[pos] = 0;
            if(pos == 0)
            {
                digits.insert(digits.begin(), 1);
            }
            else plus(digits, pos -1);
            // digits[pos] = 0;//不能在此处修改 样例[9]会修改成[09]
        }
        else digits[pos] += 1;
    }
};
```

#### while循环**

```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        while(n && ++digits[--n] == 10) digits[n] = 0;
        if(digits[0] == 0) digits.insert(begin(digits), 1);
        return digits;
    }
};
```

