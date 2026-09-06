  ##  All Complexities

# Time Complexities

| Complexity | Name        | Example Operation        |
|------------|-------------|--------------------------|
| O(1)       | Constant    | Array access             |
| O(log N)   | Logarithmic | Binary search            |
| O(N)       | Linear      | Scanning an array        |
| O(N log N) | Linearithmic| Merge sort               |
| O(N²)      | Quadratic   | Bubble sort              |
| O(N³)      | Cubic       | Matrix multiplication    |
| O(2^N)     | Exponential | Recursive Fibonacci      |
| O(N!)      | Factorial   | Traveling salesman brute |
    
  ## **O(1) Time complexity**

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

------------------------------------------------------------

**O(N2) Space Complexity and Time Complexity**

```
public static List<String> generatePairs(int[] arr) {
        List<String> pairs = new ArrayList<>();

        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr.length; j++) {
                pairs.add("(" + arr[i] + "," + arr[j] + ")");
            }
        }
        return pairs;
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


