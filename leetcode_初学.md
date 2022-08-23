

# 简单题



## 1——001两数之和

### 解法一：map解法

#### 错误点：

```java
//map的添加方法是
map.put(key,value);//非map.add(key);
//判断map是否存在某个key
map.containsKey(key);//非map.contains(key);
```

#### 代码（耗时1s

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //        键值对形式
        Map<Integer,Integer>map=new HashMap<>();
//        遍历数组
        for(int i=0;i<nums.length;i++){
//            求差
            int temp=target-nums[i];
//            查找该差值是否已知
            if(map.containsKey(temp)){
//                找到了，返回下标
                return new int[]{i,map.get(temp)};
            }
            else{
//                未找到则存入当前遍历的数值和下标
                map.put(nums[i],i);
            }
        }
//        遍历完数值仍然没找到则返回空对象
        return null;

    }
}
```



## 2——009回文数

### 解法一:StringBuffer的reverse

反转整个数再进行判断也可以，而且代码更简洁，因为如果反转溢出，那他一定不是回文数，回文数反转以后一定是它本身，所以一定不会溢出

##### 错误点：

```java
//StringBuffer非BufferString
//x是int类型
StringBuffer str=new StringBuffer(String.valueOf(x));
```

##### 代码（耗时7s

```java
class Solution {
    public boolean isPalindrome(int x) {
        String str=String.valueOf(x);
        // System.out.println(str);
        StringBuffer b_str=new StringBuffer(str);
        // System.out.println(b_str);
        return b_str.reverse().toString().equals(str);
    }
}
```

### 解法二：字符串的遍历

##### 代码（耗时5s

```java
class Solution {
    public boolean isPalindrome(int x) {
        String str=String.valueOf(x);
        for(int i=0,j=str.length()-1;i<str.length()/2;i++,j--){
            if(str.charAt(i)!=str.charAt(j)){
                return false;
            }
        }
        return true;
    }
}
```

#### 解法三：数的1/2位比大小

##### 代码（耗时4s

```java
class Solution {
    public boolean isPalindrome(int x) {
        // 特殊情况：
        // 如上所述，当 x < 0 时，x 不是回文数。
        // 同样地，如果数字的最后一位是 0，为了使该数字为回文，
        // 则其第一位数字也应该是 0
        // 只有 0 满足这一属性
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == revertedNumber || x == revertedNumber / 10;
    }
}


```

## 3——013罗马数字转整数

## 4——014最长公共前缀

### 解法一：暴力求解（双重for循环）

##### 易错点

```java
//第一点
//字符串的长度
str.length;
//第二点
//数组的长度
nums.length()
//第三点：双重遍历的时候忘记考虑越界的问题
//第四点：字符数组转换为字符串的方法
```

##### 代码（耗时1s

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0){
            return "";
        }
        else if(strs.length==1){
            return strs[0];
        }
        else{
            char[]arr=new char[strs[0].length()];
            int k=0,i=0,j=1;
            boolean flag=true;
            for(;i<strs[0].length()&&flag;i++){
                char ch=strs[0].charAt(i);
                for(j=1;j<strs.length;j++){
                    if(i>=strs[j].length()){
                        flag=false;
                        break;
                    }
                    else{
                        if(ch!=strs[j].charAt(i)){
                            flag=false;
                            break;
                        }
                    }
                }
                if(j>=strs.length){
                    arr[k++]=ch;
                }
            }
            // System.out.println(k);
            return new String(arr).substring(0,k);
        }
    }  
}
```

## 5——020有效的括号

### 解法一：栈

##### 知识点

```java
//栈的新建
Stack<Integer>stack=new Stack<>();
//栈的存入
stack.push(num);
//栈的取出
int num=stack.pop();
//栈的查看
int num=stack.peek();
```

##### 代码（耗时2ms

```java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0){
            // System.out.println("1");
            return true;
        }
        if(s.length()%2!=0||s.charAt(0)==']'||s.charAt(0)=='}'||s.charAt(0)==')'){
            // System.out.println("2");
            return false;
        }
        // 栈的特点：后进先出
        Stack<Character> stack=new Stack<>();
        for(int i=0;i<s.length();i++){
            char ch=s.charAt(i);
            if(stack.isEmpty()&&s.substring(i).length()%2!=0||s.charAt(0)==']'||s.charAt(0)=='}'||s.charAt(0)==')'){
                // System.out.println("3-");
                return false;
            }
            else if(stack.isEmpty()){
                stack.push(ch);
            }
            else{
                char c=stack.peek();
                // System.out.println("4-"+c);
                if(c=='['&&ch==']'||c=='('&&ch==')'||c=='{'&&ch=='}'){
                    stack.pop();
                }
                else{
                    stack.push(ch);
                }
            }
        }
        if(stack.isEmpty()){
            // System.out.println("4-");
            return true;
        }
        return false;
    }
}
```

### 优化代码(耗时1ms

```java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0){
            return true;
        }
        //    考察知识点：栈
        // 先判断字符串的长度，奇数一定不是有效的
        // 记录字符串的长度
        int length=s.length();
        // 判断是否为奇数
        if(length%2!=0||s.charAt(0)==']'||s.charAt(0)=='}'||s.charAt(0)==')'){
            // 奇数
            return false;
        }
        else{
            // 偶数
            // 创建栈
            // 注意要用包装类Character
            Stack<Character>stack=new Stack<Character>();
            // 遍历字符串
            for(int i=0;i<length;i++){
                // 记录当前遍历的字符串
                char ch=s.charAt(i);
                // 如果是(、{、[则入栈
                if(ch=='('||ch=='{'||ch=='['){
                    // 入栈push()
                    stack.push(ch);
                }
                // 如果不是,则要取栈顶字符判断
                else{
                    /*
                        易忘记考虑情况一:
                        }[]
                    */
                    // 如果为空栈
                    if(stack.size()==0){
                        return false;
                    }
                    // 不是空栈,判断字符是否相同
                    else if(stack.size()!=0){
                        // 取栈顶字符pop()
                        char t=stack.pop();
                        // 判断是否是[],{},()对应
                        if(t=='('&&ch!=')'||t=='['&&ch!=']'||t=='{'&&ch!='}'){
                            // 不是
                            return false;
                        }
                        else{
                            // 是则继续遍历
                            continue;
                        }
                    }
                }
            }
            /*
            易忘记考虑情况地方二:
            ")}"
            */
            // 判断是否为空栈
            if(stack.size()==0){
            //     // 不是空栈
                return true;
            }
            // else{
            //     // 是空栈
            //     return true;
            // }
        }
        return false;
    }
}
```

## 6——021合并两个有序链表

### 解法一：暴力求解

##### 代码(耗时0ms

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode list=new ListNode();
        ListNode node=list; 
        //善用&&和||
        while(list1!=null&&list2!=null){
            if(list1.val<=list2.val){
                node.next=list1;
                list1=list1.next;
            }
            else{
                node.next=list2;
                list2=list2.next;
            }
            node=node.next;
        }
        if(list1==null){
                node.next=list2;
            }
            else if(list2==null){
                node.next=list1;
            }
        return list.next;
    }
}
```

## 7——026删除有序数组中的重复数组

### 解法一:双指针

##### 代码(耗时1ms

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i=0,j=0;
        for(;i<nums.length;i++){
            if(i==0){
                nums[j++]=nums[i];
            }
            else{
                if(nums[i]!=nums[j-1]){
                    nums[j++]=nums[i];
                }
            }
        }
        return j;
    }
}

```

### 优化代码（耗时0ms

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length<=1){
            return nums.length;
        }
        else{
            int i=1,j=1;
            for(;i<nums.length;i++){
                if(nums[i]!=nums[j-1]){
                    nums[j++]=nums[i];
                }
            }
            return j;
        }
    }
}
```

## 8——027移除元素

### 解法一：双指针

##### 代码（耗时0ms

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i=0,j=0;
        for(;i<nums.length;i++){
            if(nums[i]!=val){
                nums[j++]=nums[i];
            }
        }
        return j;
    }
}
```



## 9——028实现strStr()

### 解法一：暴力求解

##### 知识点

```java
//字符串contains和indexOf方法
//判断字符串haystack是否存在子串needle
boolean flag=haystack.contains(needle);
//查找haystack第一次出现needle的第一个字符出现的位置
int location=haystack.indexOf(needle);
```

##### 代码（耗时0ms

```java
class Solution {
    public int strStr(String haystack, String needle) {
        
        if(haystack.length()<needle.length()){
            return -1;
        }
        else if(haystack.contains(needle)){
            return haystack.indexOf(needle);
        }
        else{
            return -1;
        }
    }
}
```

## 10——035搜索插入位置

### 解法一：二分查找

##### 代码（耗时0ms

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        // 二分查找
        int length=nums.length;
        if(length<=0){
            return 0;
        }
        else{
            int low=0,high=length-1;
            int mid=0;
            while(low<=high){
                mid=(low+high)/2;
                //  System.out.println(mid);
                if(nums[mid]<target){
                    if(mid+1<=length-1)
                    low=mid+1;
                    else{
                        break;
                    }
                }
                else if(target<nums[mid]){
                    if(mid-1>=0)
                    high=mid-1;
                    else{
                        break;
                    }
                }
                else{
                    return mid;
                }
            }
                if(nums[mid]>target){
                    return mid;
                }
                else{
                    return mid+1;
                }
            
        }
    }
}
```

### 代码优化

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low=0;
        int high=nums.length-1;
        while(low<=high){
            int mid=(low+high)/2;
            if(nums[mid]>target){
                if(mid-1>=0&&nums[mid-1]>=target){
                    high=mid-1;
                }
                else{
                    return mid;
                }
                
            }
            else if(nums[mid]<target){
                if(mid+1<=nums.length-1&&nums[mid+1]<=target){
                    low=mid+1;
                }
                else{
                    return mid+1;
                }
            }
            else{
                return mid;
            }
        }
        return -1;
    }
}
```

## 11——058最后一个单词的长度

### 解法一：s.substring(start,end)[start,end)

##### 代码（耗时0ms）

```java
class Solution {
    //忘记考虑最后的空格不止一个
    // 例子:"   fly me   to   the moon  "
    // 为了排除这一个情况的考虑:再倒序寻找空格的时候,应该以第一个不是空格的字符为结束条件
    public int lengthOfLastWord(String s) {
        // 判断是否有空格
        // 没空格
        if(s.indexOf(" ")==-1){
            return s.length();
        }
        // 有空格
        else{
            // 第一步：寻找空格
            int i=s.length()-1;
            for(;i>=0;i--){
                char ch=s.charAt(i);
                if(ch!=' '){
                    break;
                }
            }
            // System.out.println("第一个在:"+i);
            // 第二步：截取倒数第一个空格前的字符串str
            String str=s.substring(0,i+1);
            // 第三步：判断str是否存在空格
            int location=str.lastIndexOf(" ");
            // System.out.println("第er个在:"+location);
            // 不存在空格
            if(location==-1){
                return str.length();
            }
            // 存在
            else{
                return str.substring(location+1).length();
            }
        }
        
    }
}
```

### 解法二：s.trim（）

##### 代码（耗时0ms

```java
class Solution {
    public int lengthOfLastWord(String s) {
        /*
        题目信息：
        最后一个单词的长度
        单词 是指仅由字母组成、不包含任何空格字符的最大子字符串。
        */
        // 首先同trim() 方法用于删除字符串的头尾空白符。
        String str=new String(s.trim());
        // 首先判断字符串是否还存在空格
        if(!str.contains(" ")){
            // 不存在空格
            // 仅一个单词
            return str.length();
        }
        else{
            // 存在空格
            // 多个单词
            // 记录最后一个单词的长度
            int length=1;
            // 逆序遍历
            for(int i=str.length()-2;i>=0;i--){
                // 判断是否遇到空格
                if(str.charAt(i)!=' '){
                    // 没有遇到
                    // 仍在一个单词的范畴
                    length++;
                }
                else{
                    // 遇到空格，结束
                    break;
                }
            }
            return length;
        }
    }
}

```

## 12——066加一

### 解法一：判断是否会产生进位

##### 代码（耗时0ms

```java
class Solution {
    public int[] plusOne(int[] digits) {
        /*信息:
                1.由整数组成的非空数组
                2.结果要求非负整数加一
                3.最高数字存放在数组的首位
                4.除了整数0外,不会以0开头
        */
        /*
        解题中错误点:
            digits[digits.length-1]++;
            return digits;
            未考虑到例子:[9]返回结果是[1,0]
        */
        // 第一步:判断最后一位是否为9(9+1会产生进位)
        // 不是9
        if(digits[digits.length-1]!=9){
            digits[digits.length-1]++;
            return digits;
        }
        // 是9
        else{
            // 末尾+1;
            int i=digits.length-1;
            // 第二步:倒序遍历,判断是否为9,是否会产生进位
            for(;i>=0;i--){
                // 是9
                if(digits[i]==9){
                    digits[i]=0;
                }
                // 非9
                else{
                    digits[i]++;
                    break;
                }
            }
            // 第三步：判断是否会产生新的位数
            // 比如：99（99+1=100)
            // 没有超过原来数组的长度
            if(i>=0){
                return digits;
            }
             // 超过原来数组的长度
            else{
                // 新建数组，默认值为0
                int arr[]=new int[digits.length+1];
                arr[0]=1;
                return arr;
            }
        }
    }
}
```

### 代码优化

```java
class Solution {
    public int[] plusOne(int[] digits) {
        // 对最后一个元素操作，注意查看是否有进位
        // 首先先检查一下数组长度
        int length=digits.length;
        // 初始化进位
        int so=0;
        // 对最后一位进行处理
        // 判断最后一位是否小于9
        if(digits[length-1]<9){
            digits[length-1]++;
        }
        else{
            digits[length-1]=0;
            so=1;
        }
        // 检查数组长度
        if(length==1&&so==0){
            return digits;
        }
        else if(length==1&&so==1){
            return new int []{1,0};
        }
        else if(length>1&&so==0){
            return digits;
        }
        else{
            for(int i=length-2;i>=0;i--){
                if(digits[i]<9){
                    digits[i]++;
                    so=0;
                    return digits;
                }
                else{
                    digits[i]=0;
                }
            }
            if(so==0){
                return digits;
            }
            else{
                int[] arr=new int [length+1];
                arr[0]=1;
                return arr;
            }
        }
    }
}
```

## 13——067二进制求和

### 解法一：暴力求解

##### 代码（耗时1ms

```java
class Solution {
    public String addBinary(String a, String b) {
        // so表示进位
        char so='0';
        // 存储二进制之和
        StringBuffer s=new StringBuffer();
        // 倒数遍历字符串
        for(int i=a.length()-1,j=b.length()-1;i>=0||j>=0;i--,j--){
            char ch_a='0', ch_b='0';
            if(i>=0){
                ch_a=a.charAt(i);
            }
            if(j>=0){
                ch_b=b.charAt(j);
            }
            if(so=='0'){
                if(i>=0&&j>=0){
                    if(ch_a=='0'&&ch_b=='0'){
                        s.append("0");
                    }
                    else if(ch_a=='0'&&ch_b=='1'||ch_a=='1'&&ch_b=='0'){
                        s.append("1");
                    }
                    else{
                        s.append("0");
                        so='1';
                    }
                }
                else if(i<0){
                    if(ch_b=='0'){
                        s.append("0");
                    }
                    else{
                        s.append("1");
                    }
                }
                else if(j<0){
                    if(ch_a=='0'){
                        s.append("0");
                    }
                    else{
                        s.append("1");
                    }

                }
            }
            else if(so=='1'){
                if(i>=0&&j>=0){
                    if(ch_a=='0'&&ch_b=='0'){
                        s.append("1");
                        so='0';
                    }
                    else if(ch_a=='0'&&ch_b=='1'||ch_a=='1'&&ch_b=='0'){
                        s.append("0");
                        so='1';
                    }
                    else{
                        s.append("1");
                        so='1';
                    }
                }
                
                else if(i<0){
                    if(ch_b=='0'){
                        s.append("1");
                        so='0';
                    }
                    else{
                        s.append("0");

                    }
                }
                else if(j<0){
                    if(ch_a=='0'){
                        s.append("1");
                        so='0';
                    }
                    else{
                        s.append("0");
                    }

                }
            }
            
        }
        if(so=='1'){
            s.append("1");
        }
        return s.reverse().toString();
    }
}
```

## 14——069x的平方根

### 解法一

##### 代码（耗时108ms

```java
class Solution {
    //用乘法不好，容易越界
    public int mySqrt(int x) {
        if(x==0){
        return 0;}
        else if(x<4){
            return 1;
        }
        else{
            for(int i=2;;i++){
                if(x/i>=i&&x/(i+1)<i+1){
                    return i;
                }
            }
        }
    }
}
```

### 解法二（不懂）

##### 代码（耗时1ms

```java
class Solution {
    public int mySqrt(int x) {
    int l = 0, r = x;
while(l < r) {
	int mid = l + r + 1 >> 1;
	if(mid <= x / mid) l = mid;
	else r = mid - 1;
}
return l;

    }
}
```

## 15——070爬楼梯

### 解法一：动态规划

##### 代码（耗时0ms

```java
class Solution {
    public int climbStairs(int n) {
        //动态规划
        if(n<=0){
            return 0;
        }
        else{
             if(n<=2){
                return n;
            }
            else{
                int arr[]=new int[n+1];
                arr[1]=1;
                arr[2]=2;
                for(int i=3;i<=n;i++){
                    arr[i]=arr[i-1]+arr[i-2];
                }
                return arr[n];
            }
        }
    }
}
```

## 16 ——0[83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)

### 解法一：链表

##### 代码（耗时0ms

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        // 空链表或者一个结点
        if(head==null||head.next==null){
            return head;
        }
        else{
            // 指向第一个结点
            ListNode list=head;
            // 指向第二个结点
            ListNode next=head.next;
            while(next!=null){
                // 相等
                if(list.val==next.val){
                    list.next=next.next;
                }
                // 不相等
                else{
                    list=next;
                }
                next=next.next;
            }
            return head;
        }
    }
}
```

## 17——0[88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

### 解法一：双指针

##### 代码（耗时0ms

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {     
        // 第一种情况：nums2的长度为0
        if(n==0){
            return ;
        }
        // 第二种情况：nums2的长度不为0
        else{
            int k=m+n-1;
            // 同时遍历两个数组
            // 结束条件：nums2数组遍历完
            for(int i=m-1,j=n-1;j>=0;){
                // 第二种情况的一类：nums1和nums2均未遍历完
                if(i>=0&&j>=0){
                    if(nums1[i]>=nums2[j]){
                        nums1[k--]=nums1[i--];
                    }
                    else if(nums1[i]<nums2[j]){
                        nums1[k--]=nums2[j--];
                    }
                }
                // 第二种情况的另一类：nums1数组遍历完
                else if(i<0){
                    nums1[k--]=nums2[j--];
                }
            }
        }
    }
}
```

## 18——0[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

### 解法一

##### 错误代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        // 中序遍历：左根右
        List<Integer>list=new ArrayList<>();
        if(root==null){
            return list;
        }
        else{
            // 左
            while(root!=null){
                while(root.left!=null){
                    root=root.left;
                }
                list.add(root);
                while(root.right!=null){
                    
                }
            }
        }
    }
}
```

##### 正确代码

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        // 中序遍历：左根右
        // 创建链表,注意List是个接口
        List<Integer> list=new ArrayList<Integer>();
        // 判断根是否未空
        if(root==null){
            // 为空返回空链表
            return list;
        }
        // 判断左子树是否为空
        if(root.left!=null){
            // 不为空,则遍历左子树
            // 存入左子树的元素
            /*
            addAll()用于添加链表的所有元素,
            不会去重
            */
         list.addAll(inorderTraversal(root.left));
        }
        // 添加根节点的val
        list.add(root.val);
        // 判断右子树是否为空
        if(root.right!=null){
             // 不为空,则遍历右子树
            // 存入右子树的元素
            /*
            addAll()用于添加链表的所有元素,
            不会去重
            */
            list.addAll(inorderTraversal(root.right));
        }
        return list;
    }
}
```

## 19——[100. 相同的树](https://leetcode.cn/problems/same-tree/)

### 解法一

##### 代码（耗时0ms

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 根节点
        if(p==null&&q==null){
            return true;
        }
        else if(p==null&&q!=null||p!=null&&q==null){
            return false;
        }
        else  if(p.val!=q.val){
            return false;
        }
        // 左
         if(p.left==null&&q.left!=null||p.left!=null&&q.left==null){
            return false;
        }
        else if(p.left!=null&&q.left!=null){
            boolean flag=isSameTree(p.left,q.left);
            if(!flag){
                return false;
            }
        }
        // 右
         if(p.right==null&&q.right!=null||p.right!=null&&q.right==null){
            return false;
        }
         else if(p.right!=null&&q.right!=null){
            boolean flag2=isSameTree(p.right,q.right);
            if(!flag2){
                return false;
            }
        }
        return true;
    }
}
```

## 20——[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

### 解法一

##### 错误例子

```
[2,3,3,4,5,null,4]
```

##### 错误代码（耗时

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 根结点为空
        if(root==null){
            return true;
        }
        // 根节点的左右结点均为空
        else if(root.left==null&&root.right==null){
            return true;
        }
        // 根节点的左右结点右有个结点为空
        else if(root.left==null&&root.right!=null||root.left!=null&&root.right==null){
            return false;
        }
        else{
            TreeNode left=root.left;
            TreeNode right=root.right;
            boolean flag=isSame(left,right);
            return flag;
        }

    }
    public boolean isSame(TreeNode left,TreeNode right){
        if(left.val!=right.val){
            return false;
        }
        else if(left.left!=null&&right.right==null||left.left==null&&right.right!=null){
            return false;
        }
        else if(left.left!=null&&right.right!=null){
            boolean flag=isSame(left.left,right.right);
            if(!flag){
                return false;
            }
        }
        else if(left.right==null&&right.left!=null||left.right!=null&&right.left==null){
            return false;
        }
        else if(left.right!=null&&right.left!=null){
            boolean flag=isSame(left.right,right.left);
            if(!flag){
                return false;
            }
        }
        return true;
    }
}
```

##### 代码（耗时

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 根结点为空
        if(root==null){
            return true;
        }
        // 根节点的左右结点均为空
        else if(root.left==null&&root.right==null){
            return true;
        }
        // 根节点的左右结点右有个结点为空
        else if(root.left==null||root.right==null){
            return false;
        }
        else{
            TreeNode left=root.left;
            TreeNode right=root.right;
            boolean flag=isSame(left,right);
            return flag;
        }

    }
    public boolean isSame(TreeNode left,TreeNode right){
        if(left==null&&right==null){
            return true;
        }
        else if(left==null||right==null){
            return false;
        }
        else if(left.val!=right.val){
            return false;
        }
        else{
            boolean flag=isSame(left.left,right.right);
                if(flag){
                    return isSame(left.right,right.left);
                }
                else{
                    return flag;
                }
        }
    
    }
}
```

##### 代码

```java

```



# 中等题

## 002

