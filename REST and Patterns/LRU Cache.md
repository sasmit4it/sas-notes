LRU cache stands for least recently used cache. Cache will support two operations i.e. put and get.

Cache has a finite size, so for any key that has to be stored above the cache’s capacity, we have to evict item that has been added/accessed least recently. We implement this with a combination of hashmap and a queue.

So there are following cases to consider:

1. add a key when data size is less than the cache capacity
    

No issues here. Just add the key value to map, add the key to top of queue.

2. add a key when data size is greater than or equal to cache capacity
    

here we evict the last item from queue, then remove it from map. Then add new key value to top of queue, and add key value to map.

3. get key when it is present
    

Fetch key value from map. Remove the key from queue and add it again at top of the queue. O(n) complexity due to item removal from queue.

4. get key when it is not present.
    

return null.
```
public class LRUCacheImpl {
	// Hash map to store the cache data
	// key as Integer and Value as String
	HashMap<Integer, String> data = new HashMap<Integer, String>();
	// Linked list to store the order of cache access
	LinkedList<Integer> order = new LinkedList<Integer>();
	// size of the cache
	int capacity;

	LRUCacheImpl(int capacity) {
		this.capacity = capacity; // assigning the size of the cache while creating
	}

	// put - for adding new cache
	void put(int key, String val) {

		// Check if the cache is full by comparing the size of the list with capacity
		if (order.size() >= capacity) {
			// if the cache is full, then remove the last element from the order list and
			// also from the data
			// so we will get room for new cache
			int keyRemoved = order.removeLast(); // remove from order
			data.remove(keyRemoved);// remove from data
		}

		// add the new cache to top of the list and also to data
		order.addFirst(key);// add to top of the list
		data.put(key, val);// add to data

	}

	// get
	String get(int key) {
		String res = data.get(key); //get the value from map using the key
		if(res != null) {
			//if the data is present then we need to update the access order
			//move the current key to top of the access list
			//to move to top of the list first remove it and re-add it
			order.remove((Object) key);
			order.addFirst(key);
		}else {
			//if value is not present in the cache then its a cache miss
			//and we will return null
			res = null;
		}
		
		return res;

	}
```

