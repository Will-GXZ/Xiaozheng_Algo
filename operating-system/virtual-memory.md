# Virtual Memory
_update 2017-11-15 11:00:15_

---
### Memory fragmentation:
只有一部分 heap is “used”。process中每个内存块都需要比实际数据大，没有利用到的内部的空间叫做 "internal fragmentation"。process 之间的在 global memory map 中没利用到的空间叫做 external fragmentation。

Fragmentation 可以由于 fixed page size, buddy system, 和 reclamation 造成。我们没有办法 eliminate fragments，因为我们不知道 pointers 在哪里。


















