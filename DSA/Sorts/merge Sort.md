Merge sort is a divide and concur sort. In this algorithm, we keep on diving the target array into subarrays till each one hits length 1 i.e. single array. Once that happens(base case) we backtrack and start merging each element in a proper sorted order.

Steps:
1. Keep on dividing the array recursively till each temp array has exact one element
2. Merging will start from extreme left of the tree formed.
3. At the bottom of the tree, all the elements from the input array will be distributed across the nested stack, so you will use input array as the container only and won't care about any values in it.
4. Start merging from left to right into the input array.

**Space Complexity:**
Since it's a divide and concur algo, we divide target into two branches , and process one branch at a time. In each branch we create temp arrays of log(N) length, so peak memory which is newly allocated  till bottom of branch is:
			
$$\text{Peak Memory} = \frac{n}{2} + \frac{n}{4} + \frac{n}{8} + \dots + 1$$
which roughly equals to N. So for two branches it will 2N which becomes O(N).

**Time Complexity:**

1. **Splitting phase → log N**
    
    - Each time you split the array, you halve it: `[5, 2, 9, 1, 7] → [5, 2] + [9, 1, 7] → … → single elements`
        
    - The number of times you can halve an array of size N is roughly log₂ N. Example:
        
        - 8 elements → 3 levels of splitting (8 → 4 → 2 → 1).
            
        - 16 elements → 4 levels. So, **splitting depth ≈ log N**.
            
2. **Merging phase → N**
    
    - At each level, you merge all elements once. Every element participates in one comparison per level. So, total work per level = N.
        
3. **Combine them → N × log N**
    
    - There are log N levels, and each level processes N elements. Hence total work = N × log N.

```
public static void mergeSort(int[] a, int n) {
    if (n < 2) {
        return;
    }
    int mid = n / 2;
    int[] l = new int[mid];
    int[] r = new int[n - mid];

    for (int i = 0; i < mid; i++) {
        l[i] = a[i];
    }
    for (int i = mid; i < n; i++) {
        r[i - mid] = a[i];
    }
    mergeSort(l, mid);
    mergeSort(r, n - mid);

    merge(a, l, r, mid, n - mid);
}

public static void merge(
  int[] a, int[] l, int[] r, int left, int right) {
 
    int i = 0, j = 0, k = 0;
    while (i < left && j < right) {
        if (l[i] <= r[j]) {
            a[k++] = l[i++];
        }
        else {
            a[k++] = r[j++];
        }
    }
    while (i < left) {
        a[k++] = l[i++];
    }
    while (j < right) {
        a[k++] = r[j++];
    }
}


```

![](attachments/Pasted%20image%2020260906155538.png)