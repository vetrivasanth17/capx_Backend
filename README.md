# capx_Backend
# Multilevel Cache System - README

## Overview

This project implements a **Multilevel Cache System** with configurable cache levels that support different eviction policies. Each cache level can use either **LRU (Least Recently Used)** or **LFU (Least Frequently Used)** eviction policies, and the system manages the data across multiple levels. When data is accessed, it is promoted to higher-priority cache levels to ensure optimal access speed.

## Features

1. **Multilevel Cache Support**: The system supports multiple levels of cache (e.g., L1, L2, etc.), where each level has its own size and eviction policy.
2. **Eviction Policies**: Each cache level can be configured with either LRU or LFU eviction policies.
   - **LRU (Least Recently Used)**: Evicts the least recently accessed data when the cache is full.
   - **LFU (Least Frequently Used)**: Evicts the least frequently accessed data when the cache is full.
3. **Data Promotion**: If data is found in a lower-priority cache (e.g., L2, L3), it is automatically promoted to higher-priority levels (closer to L1).
4. **Dynamic Cache Levels**: Cache levels can be added or removed dynamically.
5. **Efficient Retrieval**: Data retrieval searches the cache levels from the highest priority (L1) downwards.

## Class Structure

### 1. **CacheLevel Class**

This class represents an individual cache level.

#### Attributes:
- `size`: Maximum number of key-value pairs the cache can hold.
- `eviction_policy`: The eviction policy for the cache (`LRU` or `LFU`).
- `cache`: An `OrderedDict` used to maintain key-value pairs. In the case of `LRU`, the order reflects access order.
- `access_data`: A `defaultdict` that tracks access frequency for the `LFU` eviction policy.

#### Methods:
- `get(key)`: Retrieves the value associated with the given key. Updates cache access details based on the eviction policy.
- `put(key, value)`: Inserts or updates the key-value pair. If the cache is full, it triggers the eviction process.
- `evict()`: Handles eviction based on the cache's eviction policy (LRU or LFU).

### 2. **MultilevelCacheSystem Class**

This class manages multiple cache levels and controls data insertion, retrieval, and promotion between levels.

#### Attributes:
- `levels`: A list of `CacheLevel` instances representing each level of the cache.

#### Methods:
- `addCacheLevel(size, eviction_policy)`: Adds a new cache level with a specified size and eviction policy.
- `get(key)`: Retrieves a value from the cache system by checking from the highest to the lowest cache level. If found in lower levels, the value is promoted to higher levels.
- `put(key, value)`: Inserts a key-value pair into the highest priority cache level (L1).
- `removeCacheLevel(level)`: Removes a specified cache level by index.
- `displayCache()`: Displays the current state of each cache level (keys and values).

## Usage

### Example

```python
# Initialize a new multilevel cache system
cache_system = MultilevelCacheSystem()

# Add L1 cache with size 3 and LRU policy
cache_system.addCacheLevel(3, 'LRU')

# Add L2 cache with size 2 and LFU policy
cache_system.addCacheLevel(2, 'LFU')

# Insert key-value pairs into the cache
cache_system.put("A", "1")
cache_system.put("B", "2")
cache_system.put("C", "3")

# Retrieve the value of key "A" from cache
cache_system.get("A")  # Returns "1" and promotes "A" to L1

# Insert key "D", which will cause eviction in L1 (based on LRU policy)
cache_system.put("D", "4")

# Retrieve the value of key "C", promotes from L2 to L1
cache_system.get("C")

# Display the current cache state
cache_system.displayCache()
```

### Expected Output:

```
L1 Cache: [('A', '1'), ('D', '4'), ('C', '3')]
L2 Cache: [('B', '2')]
```

## Installation

To use this code:

1. Copy and paste the provided code into a Python file.
2. Ensure you have Python 3.x installed.

No additional dependencies are required since the code only uses built-in Python libraries (`collections.OrderedDict` and `collections.defaultdict`).

## Explanation of Key Concepts

### 1. **Eviction Policies**
   - **LRU (Least Recently Used)**: In this policy, when the cache reaches its maximum size, the least recently accessed item is removed to make room for new data.
   - **LFU (Least Frequently Used)**: In this policy, the item with the least number of accesses is removed when the cache reaches capacity.

### 2. **Cache Levels**
   - Cache levels (L1, L2, etc.) are prioritized with L1 being the highest priority. Data retrieval begins from L1 and proceeds downwards until the data is found or the cache is exhausted. Data found in lower levels is promoted to higher levels.

### 3. **Data Promotion**
   - When data is found in a lower-priority cache (e.g., L2 or L3), it is moved (promoted) to higher-priority caches (e.g., L1) to improve future access speed.

## Customization

- **Cache Size**: You can set different sizes for different cache levels.
- **Eviction Policy**: Each cache level can use either `LRU` or `LFU` eviction policies, depending on your needs.

## Conclusion

The **Multilevel Cache System** efficiently manages data across multiple cache levels, supports both LRU and LFU eviction policies, and ensures that frequently accessed data is quickly retrievable from higher-priority caches.

Feel free to modify the code to fit your specific use case!
