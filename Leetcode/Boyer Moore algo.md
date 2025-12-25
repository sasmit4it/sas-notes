This algorithm is used to find majority elements in given unsorted array. It is necessary that the array must have one perticular elemement as meajority, otherwise algo does not work.

pseudo code:

1. set first element as final answer (res), set counter to 0
    
2. increment pointer from second element to end of array
    
3. check if currunt element == res, if yes counter++
    
4. if no counter--
    
5. after step 4 , check if counter < 0, if yes set res == current elelement, do nothing otherwise
    
6. for every reset of res, reset the counter as well
    

```
` public int majorityElement(int[] nums) {
        int res = nums[0];
        int count = 1;

        for(int i = 1 ;i<nums.length; i++){
            if(nums[i] == res){
                count++;
            }
            else{
                count--;
            }
            if(count < 0 ){
                res=nums[i];
                count = 0;
            }
        }
        return res;
    }
```