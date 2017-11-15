# Virtual Memory
_update 2017-11-15 11:00:15_

---
### Memory fragmentation:
只有一部分 heap is “used”。process中每个内存块都需要比实际数据大，没有利用到的内部的空间叫做 "internal fragmentation"。process 之间的在 global memory map 中没利用到的空间叫做 external fragmentation。

Fragmentation 可以由于 fixed page size, buddy system, 和 reclamation（process exit，heap 释放，留下fragment） 造成。我们没有办法 eliminate fragments，因为我们不知道 pointers 在哪里。

### Memory mapping:
* A frame is a unit of memory in the OS;
* A page is a unit of memory  in the process;
* Memory mapping associates (os) frames with (process) pages;
* The OS has to maintain data on what gets mapped where;

#### --> OS needs to remember
* What frame goes with what process/page?
* Special handling instructions for pages: read-only,shared, private,...
<img src="/assets/Screen Shot 2017-11-15 at 11.42.56 AM.png" width="700" height="580" /><br>
<img src="/assets/Screen Shot 2017-11-15 at 12.13.57 PM.png" width="600" height="580" /><br>
<img src="/assets/Screen Shot 2017-11-15 at 12.17.55 PM.png" width="630" height="580" />




















