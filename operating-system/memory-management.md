# Memory Management
_2017-11-14 23:18:54_

---
### Memory 的作用
1. **Internal tables:**  
  1. **File descriptors:** Describe how to communicate with devices;
  2. **Process/thread descriptors:** Describe how to run a process;
  3. **Semaphores and mutexes:** Describe the locations of shared memory;
  4. **File cache:** Describe what is on disk;
2. **Application memory:**
  1. **private:** for this application only;
  2. **shared:** (e.g., between threads);
  3. **copy on write:** when forking;
  
### Tiers of memory:
<img src="/assets/Screen Shot 2017-11-15 at 12.41.53 AM.png" width="510" height="200" />
<br>

### 命名
<img src="/assets/Screen Shot 2017-11-15 at 12.59.43 AM.png" width="600" height="500" />
<br>
**Page:** page 是 process's version of the memory;  
**Frame:** frame 是 os's view of the memory;

### The life cycle of logical memory:
<img src="/assets/Screen Shot 2017-11-15 at 2.07.47 AM.png" width="600" height="500" />
<br>

### Malloc:
<img src="/assets/Screen Shot 2017-11-15 at 2.12.03 AM.png" width="700" height="500" />
<br>

#### --> Malloc 的行为
Malloc 记录两个结构：现在正在使用的block的description以及可以被重用的block的description。为了简便，理解为 **free list** 和 **used list**；

**当调用 malloc 时**，如果 free list 中存在一个足够大的block的descriptor，则把这个descriptor 从 free list 中移动到 used list 中，并将指针返回。如果没有，就向操作系统要更多内存。

**当调用 free 时**，在 used list 中找到 block's descriptor，将其放入 free list；

**Effectively：** 所有之前已经 freed 的内存可以直接被 malloc 继续分配。所以只需要在free list 中的 descriptor 不够的时候才会 call sbrk（to add another page）；











  
  
