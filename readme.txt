Team members
CSCI 4707 LAB 3 

Sarin Madarasmi - madar007@umn.edu -madar007
Stephen Oluwaniyi - Oluwa007@umn.edu - oluwa007

Paths of Source Code Modified
src/backend/storage/buffer/freelist.c
src/backend/storage/buffer/bufmgr.c
src/backend/storage/buffer/buf_init.c
src/include/storage/buf_internals.h

#line 23 - To set The Lru replacement policy flag set 
boolLRUflag = false;

#Line 23 -To set The clock replacement policy flag set 
boolLRUflag = true;

#in our report and test, we used shared_buffers size as 1 MB ( 127 buffer indexes ) to ensure buffer replacement was used. 

#Brief Description of how the code was modified
In bufmgr.c, whenever a buffer is unpinned, we check if the pin count became zero. If so, we want to add that to the LRU queue. 
We implemented the queue as a doubly linked list by adding next and prev pointers to the Buffer Descriptor struct. When a buffer's
pin count become zero and it gets added to the queue, we check if that buffer is already in the queue. If it is, then we remove it
and add it to the end of queue again. This is because it means that the buffer was pinned and unpinned again so it was recently used. 
If it was not in the queue, then we just add the buffer to the end of the queue. In our LRU implementation, we remove the first buffer
in the queue that has zero refcount. If we find buffer in the queue that does not have refcount of 0, we just remove that from the queue
and keep going until we find the first buffer in queue with 0 refcount and return that. If no buffer exists, return NULL and there is 
no buffer available to use. 
