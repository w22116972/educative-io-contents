# Grokking Dynamic Programming Patterns for Coding Interviews



## Pattern 1: 0/1 Knapsack

1. [0/1 Knapsack Problem](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)
2. [Equal Subset Sum Partition](https://www.geeksforgeeks.org/partition-problem-dp-18/)
3. [Subset Sum](https://www.geeksforgeeks.org/subset-sum-problem-dp-25/)
4. [Minimum Subset Sum Difference](https://www.geeksforgeeks.org/partition-a-set-into-two-subsets-such-that-the-difference-of-subset-sums-is-minimum/)
5. [Count of subset sum](https://www.geeksforgeeks.org/perfect-sum-problem-print-subsets-given-sum/)
6. [Target Sum (Leetcode)](https://leetcode.com/problems/target-sum/solution/)



## Pattern 2: Unbounded Knapsack

1. [Unbounded Knapsack](https://www.geeksforgeeks.org/unbounded-knapsack-repetition-items-allowed/)
2. [Rod Cutting](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/)
3. [Coin Change](https://www.geeksforgeeks.org/coin-change-dp-7/)
4. [Minimum Coin Change](https://www.geeksforgeeks.org/find-minimum-number-of-coins-that-make-a-change/)
5. [Maximum Ribbon Cut](https://www.geeksforgeeks.org/maximum-number-segments-lengths-b-c/)



## Pattern 3: Fibonacci Numbers

1. [Fibonacci Number](https://www.geeksforgeeks.org/program-for-nth-fibonacci-number/)
2. [Staircase](https://www.geeksforgeeks.org/count-ways-reach-nth-stair/)
3. [Number divisors](https://www.geeksforgeeks.org/total-number-divisors-given-number/) - // TODO
4. [Minimum jumps to reach end](https://www.geeksforgeeks.org/minimum-number-of-jumps-to-reach-end-of-a-given-array/)
5. Minimum jumps with fee - // TODO

Problem Statement #
Given a staircase with ‘n’ steps and an array of ‘n’ numbers representing the fee that you have to pay if you take the step. Implement a method to calculate the minimum fee required to reach the top of the staircase (beyond the top-most step). At every step, you have an option to take either 1 step, 2 steps, or 3 steps. You should assume that you are standing at the first step.
Example 1:

Number of stairs (n) : 6
Fee: {1,2,5,2,1,2}
Output: 3
Explanation: Starting from index '0', we can reach the top through: 0->3->top
The total fee we have to pay will be (1+2).
Example 2:

Number of stairs (n): 4
Fee: {2,3,4,5}
Output: 5
Explanation: Starting from index '0', we can reach the top through: 0->1->top
The total fee we have to pay will be (2+3).
### Basic Solution
```
class Staircase {

  public int findMinFee(int[] fee) {
    return findMinFeeRecursive(fee, 0);
  }
  
  private int findMinFeeRecursive(int[] fee, int currentIndex) {
    if( currentIndex > fee.length - 1)
      return 0; 

    // if we take 1 step, we are left with 'n-1' steps;
    int take1Step = findMinFeeRecursive(fee, currentIndex + 1);
    // similarly, if we took 2 steps, we are left with 'n-2' steps;
    int take2Step = findMinFeeRecursive(fee, currentIndex + 2);
    // if we took 3 steps, we are left with 'n-3' steps;
    int take3Step = findMinFeeRecursive(fee, currentIndex + 3);
    
    int min = Math.min(Math.min(take1Step, take2Step), take3Step);

    return min + fee[currentIndex];
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    int[] fee = {1,2,5,2,1,2};
    System.out.println(sc.findMinFee(fee));
    fee = new int[]{2,3,4,5};
    System.out.println(sc.findMinFee(fee));
  }
}
```

### Top-down Dynamic Programming with Memoization
```java
class Staircase {

  public int findMinFee(int[] fee) {
    int dp[] = new int[fee.length];
    return findMinFeeRecursive(dp, fee, 0);
  }
  
  private int findMinFeeRecursive(int[] dp, int[] fee, int currentIndex) {
    if( currentIndex > fee.length - 1)
      return 0; 

    if(dp[currentIndex] == 0) {
      // if we take 1 step, we are left with 'n-1' steps;
      int take1Step = findMinFeeRecursive(dp, fee, currentIndex + 1);
      // similarly, if we took 2 steps, we are left with 'n-2' steps;
      int take2Step = findMinFeeRecursive(dp, fee, currentIndex + 2);
      // if we took 3 steps, we are left with 'n-3' steps;
      int take3Step = findMinFeeRecursive(dp, fee, currentIndex + 3);
  
      dp[currentIndex] = fee[currentIndex] + Math.min(Math.min(take1Step, take2Step), take3Step);
    }

    return dp[currentIndex];
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    int[] fee = {1,2,5,2,1,2};
    System.out.println(sc.findMinFee(fee));
    fee = new int[]{2,3,4,5};
    System.out.println(sc.findMinFee(fee));
  }
}
```

### Bottom-up Dynamic Programming #
Let’s try to populate our dp[] array from the above solution, working in a bottom-up fashion. As we saw in the above code, every findMinFeeRecursive(n) is the minimum of the three recursive calls; we can use this fact to populate our array.

```java
class Staircase {

  public int findMinFee(int[] fee) {
    int dp[] = new int[fee.length + 1]; // +1 to handle the 0th step
    dp[0] = 0; // if there are no steps, we dont have to pay any fee
    dp[1] = fee[0]; // only one step, so we have to pay its fee
    // for 2 or 3 steps staircase, since we start from the first step so we have to pay its fee
    // and from the first step we can reach the top by taking two or three steps, so we don't
    // have to pay any other fee.
    dp[2] = dp[3] = fee[0];

    for (int i = 3; i < fee.length; i++)
      dp[i + 1] = Math.min(fee[i] + dp[i], Math.min(fee[i - 1] + dp[i - 1], fee[i - 2] + dp[i - 2]));

    return dp[fee.length];
  }

  public static void main(String[] args) {
    Staircase sc = new Staircase();
    int[] fee = { 1, 2, 5, 2, 1, 2 };
    System.out.println(sc.findMinFee(fee));
    fee = new int[] { 2, 3, 4, 5 };
    System.out.println(sc.findMinFee(fee));
  }
}

```

6. [House Thief](https://www.geeksforgeeks.org/find-maximum-possible-stolen-value-houses/)



## Pattern 4: Palindromic Subsequence

1. [Longest Pallindromic Subsequence](https://www.geeksforgeeks.org/longest-palindromic-subsequence-dp-12/)
2. [Longest Pallindromic Substring](https://www.geeksforgeeks.org/longest-palindrome-substring-set-1/)
3. [Count of Pallindromic Substrings](https://www.geeksforgeeks.org/count-palindrome-sub-strings-string/)
4. [Minimum deletions to make a string pallindrome](https://www.geeksforgeeks.org/minimum-number-deletions-make-string-palindrome/)
5. [Pallindromic Partitioning](https://www.geeksforgeeks.org/palindrome-partitioning-dp-17/)



## Pattern 5: Longest Common Substring

1. [Longest Common Substring](https://www.geeksforgeeks.org/longest-common-substring-dp-29/)
2. [Longest Common Subsequence](https://www.geeksforgeeks.org/longest-common-subsequence-dp-4/)
3. [Minimum Deletions and Insertions to Transform a String into another](https://www.geeksforgeeks.org/minimum-number-deletions-insertions-transform-one-string-another/)
4. [Longest Increasing Subsequence](https://www.geeksforgeeks.org/longest-increasing-subsequence-dp-3/)
5. [Maximum Sum Increasing Subsequence](https://www.geeksforgeeks.org/maximum-sum-increasing-subsequence-dp-14/)
6. [Shortest Common Supersequence](https://www.geeksforgeeks.org/shortest-common-supersequence/)
7. [Minimum deletions to make sequence sorted](https://www.geeksforgeeks.org/minimum-number-deletions-make-sorted-sequence/)
8. [Longest repeating subsequence](https://www.geeksforgeeks.org/longest-repeating-subsequence/)
9. Subsequence Pattern Matching  - // TODO

Problem Statement #
Given a string and a pattern, write a method to count the number of times the pattern appears in the string as a subsequence.

Example 1: Input: string: “baxmx”, pattern: “ax”
Output: 2
Explanation: {baxmx, baxmx}.

Example 2:

Input: string: “tomorrow”, pattern: “tor”
Output: 4
Explanation: Following are the four occurences: {tomorrow, tomorrow, tomorrow, tomorrow}.

### Basic Solution #
This problem follows the Longest Common Subsequence (LCS) pattern and is quite similar to the Longest Repeating Subsequence; the difference is that we need to count the total occurrences of the subsequence.

A basic brute-force solution could be to try all the subsequences of the given string to count all that match the given pattern. We can match the pattern with the given string one character at a time, so we can do two things at any step:

If the pattern has a matching character with the string, we can recursively match for the remaining lengths of the pattern and the string.
At every step, we can always skip a character from the string to try to match the remaining string with the pattern. So we can start a recursive call by skipping one character from the string.
Our total count will be the sum of the counts returned by the above two options.

```java
class SPM {

  public int findSPMCount(String str, String pat) {
    return findSPMCountRecursive(str, pat, 0, 0);
  }

  private int findSPMCountRecursive(String str, String pat, int strIndex, int patIndex) {

    // if we have reached the end of the pattern
    if(patIndex == pat.length())
      return 1;

    // if we have reached the end of the string but pattern has still some characters left
    if(strIndex == str.length())
      return 0;

    int c1 = 0;
    if(str.charAt(strIndex) == pat.charAt(patIndex))
      c1 = findSPMCountRecursive(str, pat, strIndex+1, patIndex+1);

    int c2 = findSPMCountRecursive(str, pat, strIndex+1, patIndex);

    return c1 + c2;
  }

  public static void main(String[] args) {
    SPM spm = new SPM();
    System.out.println(spm.findSPMCount("baxmx", "ax"));
    System.out.println(spm.findSPMCount("tomorrow", "tor"));
  }
}
```

### Top-down Dynamic Programming with Memoization #
We can use an array to store the already solved subproblems.

The two changing values to our recursive function are the two indexes strIndex and patIndex. Therefore, we can store the results of all the subproblems in a two-dimensional array. (Another alternative could be to use a hash-table whose key would be a string (strIndex + “|” + patIndex)).

```java
class SPM {

  public int findSPMCount(String str, String pat) {
    Integer[][] dp = new Integer[str.length()][pat.length()];
    return findSPMCountRecursive(dp, str, pat, 0, 0);
  }

  private int findSPMCountRecursive(Integer[][] dp, String str, String pat, int strIndex, int patIndex) {

    // if we have reached the end of the pattern
    if(patIndex == pat.length())
      return 1;

    // if we have reached the end of the string but pattern has still some characters left
    if(strIndex == str.length())
      return 0;

    if(dp[strIndex][patIndex] == null) {
      int c1 = 0;
      if(str.charAt(strIndex) == pat.charAt(patIndex))
        c1 = findSPMCountRecursive(dp, str, pat, strIndex+1, patIndex+1);
      int c2 = findSPMCountRecursive(dp, str, pat, strIndex+1, patIndex);
      dp[strIndex][patIndex] = c1 + c2;
    }

    return dp[strIndex][patIndex];
  }

  public static void main(String[] args) {
    SPM spm = new SPM();
    System.out.println(spm.findSPMCount("baxmx", "ax"));
    System.out.println(spm.findSPMCount("tomorrow", "tor"));
  }
}

```

### Bottom-up Dynamic Programming #
Since we want to match all the subsequences of the given string, we can use a two-dimensional array to store our results. As mentioned above, we will be tracking separate indexes for the string and the pattern, so we will be doing two things for every value of strIndex and patIndex:

If the character at the strIndex (in the string) matches the character at patIndex (in the pattern), the count of the SPM would be equal to the count of SPM up to strIndex-1 and patIndex-1.
At every step, we can always skip a character from the string to try matching the remaining string with the pattern; therefore, we can add the SPM count from the indexes strIndex-1 and patIndex.
So our recursive formula would be:

```java
if str[strIndex] == pat[patIndex] {
  dp[strIndex][patIndex] = dp[strIndex-1][patIndex-1]
}
dp[i1][i2] += dp[strIndex-1][patIndex]
```

```java
class SPM {

  public int findSPMCount(String str, String pat) {
    // every empty pattern has one match
    if(pat.length() == 0)
      return 1;
    
    if(str.length() == 0 || pat.length() > str.length())
      return 0;

    // dp[strIndex][patIndex] will be storing the count of SPM up to str[0..strIndex-1][0..patIndex-1]
    int[][] dp = new int[str.length()+1][pat.length()+1];

    // for the empty pattern, we have one matching
    for(int i=0; i<=str.length(); i++)
      dp[i][0] = 1;

    for(int strIndex=1; strIndex<=str.length(); strIndex++) {
      for(int patIndex=1; patIndex<=pat.length(); patIndex++) {
        if(str.charAt(strIndex-1) == pat.charAt(patIndex-1))
          dp[strIndex][patIndex] = dp[strIndex-1][patIndex-1];
        dp[strIndex][patIndex] += dp[strIndex-1][patIndex];
      }
    }

    return dp[str.length()][pat.length()];
  }

  public static void main(String[] args) {
    SPM spm = new SPM();
    System.out.println(spm.findSPMCount("baxmx", "ax"));
    System.out.println(spm.findSPMCount("tomorrow", "tor"));
  }
}

```



10. [Longest Bitonic Subsequence](https://www.geeksforgeeks.org/longest-bitonic-subsequence-dp-15/)
11. [Longest Alternating Subsequence](https://www.geeksforgeeks.org/longest-alternating-subsequence/)
12. [Edit Distance](https://www.geeksforgeeks.org/edit-distance-dp-5/)
13. [String Interleaving](https://www.geeksforgeeks.org/find-if-a-string-is-interleaved-of-two-other-strings-dp-33/)
14. ​
