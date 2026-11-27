# File System

In the previous sections, we've already seen two very important virtualizations:

1. CPU Virtualization: The process
2. Memory Virtualization: The address space

In this section, we add one more critical piece to the virtualization puzzle: **persistent storage**. A persistent-storage device, such as a classic HDD or SSD, stores information permanently (or at least, for a long time).

## Big Picture

Essentially, a **file system** resides on a storage device (e.g., a disk) and manages the **files and directories** stored on it. The disk is divided into **blocks**, which are the basic units of storage and disk I/O. Thus, although files and directories can be viewed as arrays of bytes, the file system typically accesses their contents in units of blocks!

* **Files:** Each file is associated with an **inode number**, which identifies its **inode**, an on-disk data structure that stores the file's metadata, such as its size, last modification time, and **data block pointers**. These pointers locate the data blocks containing the actual file contents (which are essentially an array of bytes). Typically, multiple inodes can fit within a single file-system block. Through **multi-level indexing** (e.g., indirect block pointers), a file can grow to a very large size without requiring an excessive number of pointers directly in its inode.
* **Directories:** Each directory is also associated with an inode number that identifies its inode. Similar to a regular file, a directory's inode contains data block pointers. However, these pointers lead to **"special" data blocks**, which store entries mapping **file or directory names to inode numbers**. This allows the file system to locate the inode associated with a given name.

Reading and writing files can be performed through **system calls**, such as `read()` and `write()`. However, how the file system accesses and organizes file contents on disk can be more complex (which we will explore later in our [notes](file-system-implementation.md#access-paths))!
