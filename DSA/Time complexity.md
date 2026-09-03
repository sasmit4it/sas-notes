  # **O(1) Space complexity**

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

  ## **O(N) Time Complexity**

```
public static void main(String[] args) {
        
        // Simple O(N) loop, N depends on input array's length i.e. variable
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
```
  
  O(1) Space Complexity

```
public static int calculateSum(int[] arr) {
        int sum = 0; // single variable → constant space
        for (int i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
        return sum;
    }
```

O(N) Space Complexity

```
public static int[] makeCopy(int[] arr) {
        int[] copy = new int[arr.length]; // new array of size N
        for (int i = 0; i < arr.length; i++) {
            copy[i] = arr[i];
        }
        return copy;
    }
```

O(N2) Space Complexity

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

O(N) Time and Space Complexity - Recursive

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


