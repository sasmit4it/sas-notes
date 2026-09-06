  ##  All Complexities

# Time Complexities

| Complexity | Name         | Example Operation        |
| ---------- | ------------ | ------------------------ |
| O(1)       | Constant     | Array access             |
| O(log N)   | Logarithmic  | Binary search            |
| O(N)       | Linear       | Scanning an array        |
| O(N log N) | Linearithmic | Merge sort               |
| O(N²)      | Quadratic    | Bubble sort              |
| O(N³)      | Cubic        | Matrix multiplication    |
| O(2^N)     | Exponential  | Recursive Fibonacci      |
| O(N!)      | Factorial    | Traveling salesman brute |
    
  #O(1) Time and Space Complexity

```
public class ConstantLoop {
    public static void main(String[] args) {
        int[] arr = {5, 2, 9, 1, 7};

        // O(1) loop: runs exactly 3 times, independent of array size
        for (int i = 0; i < 3; i++) {
            System.out.println("Iteration " + i);
        }
    }
}
```

  
  ## **O(1) Space Complexity**

```
public static int calculateSum(int[] arr) {
        int sum = 0; // single variable → constant space
        for (int i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
        return sum;
    }
```

------------------------------------------------------------

  ## **O(N) Time Complexity**

```
public static void main(String[] args) {
        
        // Simple O(N) loop, N depends on input array's length i.e. variable
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
```
  

  ## **O(N) Space Complexity**

```
public static int[] makeCopy(int[] arr) {
        int[] copy = new int[arr.length]; // new array of size N
        for (int i = 0; i < arr.length; i++) {
            copy[i] = arr[i];
        }
        return copy;
    }
```


 **O(N) Space and Time Complexity - Recursive**

- **Time Complexity:** O(N)
    
    - Each call reduces `n` by 1, so there are `N` recursive calls until the base case.
        
- **Space Complexity:** O(N)
    
    - Each recursive call adds a new frame to the call stack. For `n = 5`, there are 5 stack frames; for `n = 1000`, there are 1000 frames.

```
public static int factorial(int n) {
        if (n == 0 || n == 1) {
            return 1; // base case
        }
        return n * factorial(n - 1); // recursive call
    }
```

--------------------------------------------------------------

**O(log(N) Time and Space Complexity**

log(logarithmic) complexity basically means divide the target to solve problem.
Classic example is binary search where on each iteration we just remove half the array from search and focus on remaining half.

- **Time Complexity:** O(log N) → each recursive call halves the search space.
    
- **Space Complexity:** O(log N) → Example of N(LogN) S

```
public class LogNExample {
    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 8, 10};
        int target = 8;

        int left = 0, right = arr.length - 1;

        while (left <= right) {
            int mid = (left + right) / 2;

            if (arr[mid] == target) {
                System.out.println("Found at index " + mid);
                return;
            } else if (arr[mid] < target) {
                left = mid + 1; // search right half
            } else {
                right = mid - 1; // search left half
            }
        }

        System.out.println("Not found");
    }
}

```

## O(N log N) Time and Space Complexity

N log N complexity means the problem is divided into halves (log N levels), and at each level all N elements are processed.  
Classic example is **merge sort**, where the array is recursively split and then merged back.

- **Time Complexity:** O(N log N) → log N levels of splitting × N work per level.
- **Space Complexity:** O(N) → temporary arrays used during merging plus recursion stack.

```java
import java.util.Arrays;

public class MergeSortExample {
    public static void main(String[] args) {
        int[] arr = {5, 2, 9, 1, 7};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println("Sorted: " + Arrays.toString(arr));
    }

    public static void mergeSort(int[] arr, int left, int right) {
        if (left >= right) return;

        int mid = (left + right) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }

    private static void merge(int[] arr, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0;

        while (i <= mid && j <= right) {
            temp[k++] = (arr[i] <= arr[j]) ? arr[i++] : arr[j++];
        }
        while (i <= mid) temp[k++] = arr[i++];
        while (j <= right) temp[k++] = arr[j++];

        System.arraycopy(temp, 0, arr, left, temp.length);
    }
}

```

## O(N²) Time and Space Complexity

Quadratic complexity means the algorithm compares or processes every element against every other element.  
Classic example is **bubble sort**, where each pass compares adjacent elements and repeats N times.

- **Time Complexity:** O(N²) → nested loops, each element compared with all others.
- **Space Complexity:** O(1) → sorting is done in place, only a few extra variables used.

```java
import java.util.Arrays;

public class BubbleSortExample {
    public static void main(String[] args) {
        int[] arr = {5, 2, 9, 1, 7};
        bubbleSort(arr);
        System.out.println("Sorted: " + Arrays.toString(arr));
    }

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}

```

O(N²) Space Complexity

```
public class LCSExample {
    public static int lcs(String a, String b) {
        int n = a.length();
        int m = b.length();
        int[][] dp = new int[n+1][m+1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (a.charAt(i-1) == b.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[n][m];
    }
```


## O(N³) Time and Space Complexity
Just like N² , just add one more loop for time complexity and one more dimension for space complexity.

-----------------------------------------------
## $O(2^N)$ or $(3^N)$ Time and Space Complexity

Exponential Complexity although looks similar to quadratic, it is of much higher scale.

|**Input Size (n)**|**n2 (Quadratic)**|**2n (Exponential)**|
|---|---|---|
|**$n = 5$**|$25$ operations|$32$ operations|
|**$n = 10$**|$100$ operations|$1,024$ operations|
|**$n = 20$**|$400$ operations|$1,048,576$ operations|
|**$n = 50$**|$2,500$ operations|$\approx 1.12 \times 10^{15}$ operations|
- **How Code Causes It:** Unoptimized binary recursion where each step branches into 2/3 separate recursive calls without caching previously computed results.
    
- **Practical Impact:** Quickly becomes unusable for even tiny inputs (e.g., $n = 50$ requires over 1 quadrillion operations).
    
- **Common Examples:** Naive recursive Fibonacci, generating all subsets of a set (Power Set), or brute-forcing dynamic programming problems.

```
# Each call branches into 2 more calls = O(2^n)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

![](attachments/Pasted%20image%2020260906200951.png)

For every level we're doubling the number of nodes, and that happens N times.

-------------------------------------------------------

## O(N!) Time and Space Complexity

## O(N!) Time and Space Complexity

Factorial complexity means the algorithm explores **all possible permutations** of the input.  
Classic example is **generating all permutations of an array**, where each element can be placed in every position.

- **Time Complexity:** O(N!) → number of permutations grows factorially with N.
- **Space Complexity:** O(N) → recursion stack + temporary storage for current permutation.

```java
import java.util.*;

public class PermutationsExample {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        List<List<Integer>> result = new ArrayList<>();
        backtrack(arr, new boolean[arr.length], new ArrayList<>(), result);
        System.out.println("Permutations: " + result);
    }

    private static void backtrack(int[] arr, boolean[] used, List<Integer> curr, List<List<Integer>> result) {
        if (curr.size() == arr.length) {
            result.add(new ArrayList<>(curr));
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            curr.add(arr[i]);
            backtrack(arr, used, curr, result);
            curr.remove(curr.size() - 1);
            used[i] = false;
        }
    }
}

```
![](attachments/Pasted%20image%2020260906202039.png)