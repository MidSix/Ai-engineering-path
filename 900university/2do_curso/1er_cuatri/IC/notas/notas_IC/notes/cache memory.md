# Cache Memory — Conceptual Summary
[God video](https://www.youtube.com/watch?v=Bz49xnKBH_0)
## 1. What Cache Memory Is

**Cache memory** is a **small, fast memory** located between the **CPU** and **main memory (RAM)**.

Its purpose is to:
- Reduce the average memory access time
- Exploit **locality of reference**
- Avoid frequent accesses to slow main memory

In modern computers, cache is:
- Implemented **inside the CPU chip**
- Built using **semiconductor technology**
- **Volatile** (data is lost when power is off)

---

## 2. Fundamental Principle: Locality

Cache works because programs exhibit **locality**:
### Temporal locality

If a data item is accessed, it is likely to be accessed again soon.
### Spatial locality

If a data item is accessed, nearby memory addresses are likely to be accessed soon.

Therefore, cache does not fetch a single word(even when it could) but a **block of consecutive memory** in order to make a thing of Locality.

---

## 3. Lines, Blocks, and Terminology

### Cache line

A **cache line** is the basic storage unit of the cache.

It contains:

- A **data block** (multiple bytes / words)
    
- **Metadata** (tag + control bits)
    

### Main memory blocks

Main memory does **not physically have lines**.

However, **for cache purposes**, main memory is _conceptually divided_ into **blocks of the same size as a cache line**.

> A cache line stores a **copy of a block of main memory**.

So:

- **Cache line** → cache concept
    
- **Memory block** → main memory concept
    
- Same size, different roles
    

---

## 4. Cache Line Size

The **line size** is the number of bytes stored in one cache line.

- It contains **1 or more words**
    
- Minimum theoretical size = **1 word**
    
    - 32-bit CPU → 1 word = 4 B
        
    - 64-bit CPU → 1 word = 8 B
        
- In practice, line sizes are much larger:
    
    - 32 B, 64 B, 128 B, 256 B, …
        

Reason: to exploit **spatial locality**.

---

## 5. Cache as an Associative Memory

Cache is **associative** in behavior.

The CPU:

- Requests data using a **memory address**
    
- Does **not know**:
    
    - whether the data is in cache
        
    - where it would be stored inside the cache
        

The cache:

- Searches its contents using **tags**
    
- Answers:
    
    - **HIT** → data found
        
    - **MISS** → data not found, fetch from RAM
        

This is fundamentally different from main memory, which is **address-based**.

---

## 6. Cache Structure

A cache is logically divided into two parts:

### 6.1 Data storage (data array)

Stores the **actual bytes** copied from main memory.

### 6.2 Directory (metadata)

Stores information about each cache line:

- **Tag** → identifies which memory block is stored
    
- **Valid bit** → indicates whether the line contains valid data
    
- (Optionally: dirty bit, replacement info, etc.)
    

---

## 7. Address Breakdown

A physical memory address is split into fields:

Address=[Tag∣Index∣Offset]\text{Address} = [\text{Tag} \mid \text{Index} \mid \text{Offset}]Address=[Tag∣Index∣Offset]

### Offset

- Selects a **byte/word inside the cache line**
    
- Number of bits:
    

log⁡2(line size in bytes)\log_2(\text{line size in bytes})log2​(line size in bytes)

### Index

- Selects **where to look in the cache**
    
- Depends on cache organization
    

### Tag

- Identifies **which memory block** is stored in the line
    
- Compared to directory tags to detect hits/misses
    

---

## 8. Direct-Mapped Cache (Correspondencia Directa)

In a **direct-mapped cache**:

- Each memory block maps to **exactly one cache line**
    
- Mapping is deterministic
    

### Key properties

- Simple and fast
    
- Low hardware cost
    
- Higher conflict misses
    

### Number of cache lines

#lines=cache sizeline size\#\text{lines} = \frac{\text{cache size}}{\text{line size}}#lines=line sizecache size​

### Index bits

log⁡2(#lines)\log_2(\#\text{lines})log2​(#lines)

---

## 9. Cache Lookup Process (Direct-Mapped)

`flowchart TD     CPU[CPU requests address] --> Cache{Check cache}     Cache -->|Tag match & valid| Hit[Cache HIT → return data]     Cache -->|No match| Miss[Cache MISS → fetch from RAM]     Miss --> Fill[Store block in cache line]     Fill --> CPU`

Steps:

1. CPU sends address
    
2. Index selects the cache line
    
3. Tag is compared
    
4. If valid + tag match → HIT
    
5. Else → MISS, block fetched from RAM
    

---

## 10. Important Conceptual Distinctions

|Concept|Meaning|
|---|---|
|Cache line|Storage unit in cache|
|Memory block|Conceptual chunk of main memory|
|Word|CPU’s natural data size|
|Byte-addressed|1 address = 1 byte|
|Word-addressed|1 address = 1 word|
|Offset|Position inside the line|
|Tag|Identifies the memory block|
|Index|Selects cache line|

---

## 11. Key Exam-Ready Statements

- _Cache memory stores copies of blocks of main memory in cache lines._
    
- _Cache is associative because data is searched using tags, not explicit addresses._
    
- _The minimum cache line size is one word, but in practice lines contain multiple words._
    
- _In direct-mapped cache, each memory block maps to exactly one cache line._
    
- _The offset selects the byte/word inside the cache line._
    

---

## 12. Mental Model (Final)

> **Cache does not store “addresses”.  
> It stores blocks of memory, each labeled with a tag.  
> The CPU asks: “Do you have data for this address?”  
> The cache answers: hit or miss.**